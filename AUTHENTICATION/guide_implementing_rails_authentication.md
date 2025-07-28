### **Introduction**

**What is Authentication?**

Authentication is the process of verifying who a user is. It ensures that only authorized users can access certain parts of your application.

**What is a Rails API?**

Rails is a web application framework written in Ruby. A Rails API is a backend application that provides data to frontend applications or clients.

### **Prerequisites**

- Basic knowledge of Ruby and Rails.
- A Rails API application set up.

### **Step 1: Add Necessary Gems**

1. **Open Your Gemfile**

   - Located in the root directory of your Rails project.

2. **Add the Following Gems**

   ```ruby
   gem 'bcrypt', '~> 3.1.7' # For hashing passwords securely
   gem 'jwt', '~> 2.2'      # For generating JSON Web Tokens
   ```

3. **Install the Gems**

   ```bash
   bundle install
   ```

### **Step 2: Generate a User Model**

1. **Create the User Model**

   ```bash
   rails generate model User name:string email:string password_digest:string
   ```

2. **Migrate the Database**

   ```bash
   rails db:migrate
   ```

### **Step 3: Set Up User Authentication**

1. **Modify the User Model**

   - Open `app/models/user.rb`.
   - Add:

     ```ruby
     class User < ApplicationRecord
       has_secure_password
       validates :email, presence: true, uniqueness: true
     end
     ```

   - **Explanation**:
     - `has_secure_password` adds methods to set and authenticate passwords.
     - Validates that email is present and unique.

### **Step 4: Create Users Controller**

1. **Generate the Controller**

   ```bash
   rails generate controller Users
   ```

2. **Implement the Create Action**

   - Open `app/controllers/users_controller.rb`.
   - Add:

     ```ruby
     class UsersController < ApplicationController
       def create
         user = User.new(
           name: params[:name],
           email: params[:email],
           password: params[:password],
           password_confirmation: params[:password_confirmation]
         )
         if user.save
           render json: { message: 'User created successfully' }, status: :created
         else
           render json: { errors: user.errors.full_messages }, status: :unprocessable_entity
         end
       end
     end
     ```

### **Step 5: Set Up Routes**

- Open `config/routes.rb`.
- Add:

  ```ruby
  post '/signup', to: 'users#create'
  ```

### **Step 6: Implement Login Functionality**

1. **Generate Sessions Controller**

   ```bash
   rails generate controller Sessions
   ```

2. **Implement the Create Action**

   - Open `app/controllers/sessions_controller.rb`.
   - Add:

     ```ruby
     class SessionsController < ApplicationController
       def create
         user = User.find_by(email: params[:email])
         if user && user.authenticate(params[:password])
           token = JWT.encode({ user_id: user.id }, Rails.application.credentials.secret_key_base)
           render json: { jwt: token, user: user }, status: :created
         else
           render json: { error: 'Invalid email or password' }, status: :unauthorized
         end
       end
     end
     ```

   - **Explanation**:
     - Finds the user by email.
     - Authenticates the password.
     - Generates a JWT token if authentication is successful.

### **Step 7: Update Routes for Login**

- In `config/routes.rb`, add:

  ```ruby
  post '/login', to: 'sessions#create'
  ```

### **Step 8: Protecting Routes with Authentication**

1. **Implement the Current User Method**

   - Open `app/controllers/application_controller.rb`.
   - Add:

     ```ruby
     class ApplicationController < ActionController::API
       def authorize_request
         header = request.headers['Authorization']
         token = header.split(' ').last if header
         begin
           decoded = JWT.decode(token, Rails.application.credentials.secret_key_base)[0]
           @current_user = User.find(decoded['user_id'])
         rescue ActiveRecord::RecordNotFound, JWT::DecodeError
           render json: { errors: 'Unauthorized' }, status: :unauthorized
         end
       end

       def current_user
         @current_user
       end
     end
     ```

2. **Protect Controller Actions**

   - In any controller you want to protect, add:

     ```ruby
     before_action :authorize_request
     ```

### **Step 9: Test the Authentication Flow**

1. **Signup**

   - Use a tool like Postman to send a POST request to `http://localhost:3000/signup` with body parameters:

     ```json
     {
       "name": "John Doe",
       "email": "john@example.com",
       "password": "password123",
       "password_confirmation": "password123"
     }
     ```

2. **Login**

   - Send a POST request to `http://localhost:3000/login` with body parameters:

     ```json
     {
       "email": "john@example.com",
       "password": "password123"
     }
     ```

   - You'll receive a JWT token in the response.

3. **Access Protected Routes**

   - Include the JWT token in the `Authorization` header of your requests:

     ```
     Authorization: Bearer your_jwt_token
     ```

### **Conclusion**

You've set up user authentication in your Rails API using JWTs. This allows users to securely sign up, log in, and access protected resources.

---
