# Pundit and Role-Based Authorization Guide

## Introduction

**What is Authorization?**  
Authorization is about determining what an authenticated user is allowed to do. Where authentication verifies a user’s identity, authorization enforces which actions and resources they can access.

**What is Pundit?**  
Pundit is a Ruby gem that provides a clean, simple way to manage authorization logic in a Rails application. It uses **policy classes** to define what actions a user can perform on a given resource.

## Prerequisites

- A Rails API application with user authentication in place (e.g., JWT).
- Basic knowledge of Rails models, controllers, and migrations.

---

## Step 1: Add the Necessary Gem

1. **Open Your Gemfile**

   Located in the root directory of your Rails project.

2. **Add the Pundit Gem**

   ```ruby
   gem 'pundit'
   ```

3. **Install the Gem**

   ```bash
   bundle install
   ```

---

## Step 2: Add a Role to Your User Model

### 2a. Generate a Migration to Add a `role` Column

```bash
rails generate migration AddRoleToUsers role:string
```

Inside the generated migration (found in `db/migrate/...`), you can set a default role if desired:

```ruby
class AddRoleToUsers < ActiveRecord::Migration[6.1]
  def change
    add_column :users, :role, :string, default: 'user'
  end
end
```

### 2b. Migrate the Database

```bash
rails db:migrate
```

### 2c. Update the User Model

Open `app/models/user.rb` and add validation/role helpers:

```ruby
class User < ApplicationRecord
  has_secure_password

  validates :role, inclusion: { in: %w[user admin] }

  def admin?
    role == 'admin'
  end

  def user?
    role == 'user'
  end
end
```

**Explanation**  
- We’ve added a `role` column with a default value of `'user'`.
- We validate that `role` is either `'user'` or `'admin'`.
- `admin?` and `user?` helper methods make policy checks more readable.

---

## Step 3: Install and Configure Pundit

### 3a. Install Pundit Structure (Optional)

```bash
rails generate pundit:install
```

This will create an `app/policies/application_policy.rb` file for shared base logic.

### 3b. Include Pundit in Your ApplicationController

Open `app/controllers/application_controller.rb` and modify it:

```ruby
class ApplicationController < ActionController::API
  include Pundit

  rescue_from Pundit::NotAuthorizedError, with: :user_not_authorized

  private

  def user_not_authorized
    render json: { error: 'You are not authorized to perform this action.' }, status: :forbidden
  end
end
```

**Explanation**  
- `include Pundit` makes Pundit’s authorization methods (like `authorize` and `policy_scope`) available.
- `rescue_from Pundit::NotAuthorizedError` catches errors when a user isn’t allowed to do something, returning a `403 Forbidden` response.

---

## Step 4: Create a Policy

Policies define who can do what. Suppose you have a `Product` model you want to protect:

1. **Generate a Policy**

   ```bash
   rails generate pundit:policy Product
   ```

   This creates `app/policies/product_policy.rb`.

2. **Define Permissions in `product_policy.rb`**

   ```ruby
   class ProductPolicy < ApplicationPolicy
     class Scope < Scope
       def resolve
         if user.admin?
           scope.all
         else
           scope.where(user_id: user.id)
         end
       end
     end

     # Pundit expects an initialize method if we override the parent
     def initialize(user, product)
       @user = user
       @product = product
     end

     def index?
       true
     end

     def show?
       @product.user_id == @user.id || @user.admin?
     end

     def create?
       @user.present?
     end

     def update?
       @user.admin? || (@product.user_id == @user.id)
     end

     def destroy?
       @user.admin? || (@product.user_id == @user.id)
     end
   end
   ```

**Explanation**  
- `Scope` class controls **which records** the user can see when querying a collection (e.g., using `policy_scope(Product)`).
- Each method (`show?`, `update?`, etc.) returns a boolean: `true` if the user can perform that action, `false` if not.
- We check `@user.admin?` for admin privileges; otherwise, only allow actions if the product belongs to the user.

---

## Step 5: Use the Policy in Your Controller

Open (or create) `app/controllers/products_controller.rb`:

```ruby
class ProductsController < ApplicationController
  before_action :authorize_request  # Ensure the user is authenticated

  # GET /products
  def index
    # Only return the products the user is allowed to see
    products = policy_scope(Product)
    render json: products
  end

  # GET /products/:id
  def show
    product = Product.find(params[:id])
    authorize product  # calls ProductPolicy#show?
    render json: product
  end

  # POST /products
  def create
    product = Product.new(
      name: params[:name],
      description: params[:description],
      user_id: current_user.id
    )

    authorize product  # calls ProductPolicy#create?

    if product.save
      render json: product, status: :created
    else
      render json: { errors: product.errors.full_messages }, status: :unprocessable_entity
    end
  end

  # PATCH/PUT /products/:id
  def update
    product = Product.find(params[:id])
    authorize product  # calls ProductPolicy#update?

    if product.update(name: params[:name], description: params[:description])
      render json: product
    else
      render json: { errors: product.errors.full_messages }, status: :unprocessable_entity
    end
  end

  # DELETE /products/:id
  def destroy
    product = Product.find(params[:id])
    authorize product  # calls ProductPolicy#destroy?

    product.destroy
    render json: { message: 'Product deleted' }, status: :ok
  end
end
```

**Explanation**  
- `policy_scope(Product)` applies the `Scope` logic defined in `ProductPolicy`, returning only the records the user is allowed to see.  
- `authorize product` checks the action-specific method (e.g., `show?`, `update?`, `destroy?`) in the policy.  
- If the user isn’t authorized, Pundit raises a `Pundit::NotAuthorizedError`, which we rescue in `ApplicationController`.

---

## Step 6: Testing Your Role-Based Authorization

1. **Login** as a regular user (with `role: 'user'`).
2. **Try** creating, viewing, and modifying products. Verify:
   - You can only see your own products (based on the `Scope`).
   - You can’t edit or delete products that belong to someone else.
3. **Login** as an admin user (with `role: 'admin'`).
4. **Verify** that you can access or modify all products, as allowed by `admin?` checks.

Use Postman or any API client to test these endpoints with different JWT tokens.

---

## Step 7: (Optional) Creating More Roles or Custom Actions

- **More Roles**  
  If you want more than `'user'` and `'admin'`, expand your model validations or consider using an `enum`.
- **Custom Actions**  
  Have a non-standard action like `publish` or `archive`? Simply define a `publish?` or `archive?` method in `ProductPolicy`, then call `authorize @product, :publish?` in the controller.

---

## Conclusion

You’ve implemented **Pundit** to handle **role-based authorization** in your Rails API. By defining clear policy classes, you keep your authorization logic organized and secure. With roles in place (`user`, `admin`, etc.), your application can selectively grant and restrict actions, ensuring only the right people perform the right tasks.

**Key Points to Remember**  
- **Always** call `authorize` on individual records and `policy_scope` on collections.
- Keep your **policy methods** short and readable (returning true or false).
- The `role` field in `User` is crucial for differentiating privileges.

You now have a foundation for more advanced scenarios—like multiple roles, custom policy logic, and further security measures. Happy coding!