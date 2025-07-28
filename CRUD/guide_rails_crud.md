### **Introduction**

**What is CRUD?**

CRUD stands for **Create**, **Read**, **Update**, and **Delete**. These are the basic operations you can perform on data in a database.

**What is a Rails API?**

Rails can be used to create APIs, which are backend applications that provide data to frontends or other clients.

### **Prerequisites**

- Basic understanding of Ruby and Rails.
- A new Rails API application.

### **Step 1: Create a New Rails API Application**

1. **Generate a New Application**

   ```bash
   rails new my_api --api
   ```

   - The `--api` flag creates a slimmed-down version suitable for APIs.

2. **Navigate to the Project Directory**

   ```bash
   cd my_api
   ```

3. **Initialize Git (Optional but Recommended)**

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

### **Step 2: Generate the Photo Model**

1. **Create the Model**

   ```bash
   rails generate model Photo name:string width:integer height:integer
   ```

2. **Migrate the Database**

   ```bash
   rails db:migrate
   ```

### **Step 3: Seed the Database**

1. **Add Seed Data**

   - Open `db/seeds.rb`.
   - Add:

     ```ruby
     Photo.create([
       { name: 'Sunset', width: 1920, height: 1080 },
       { name: 'Mountain', width: 1280, height: 720 }
     ])
     ```

2. **Run the Seeds**

   ```bash
   rails db:seed
   ```

### **Step 4: Create the Controller**

1. **Generate the Controller**

   ```bash
   rails generate controller Photos
   ```

### **Step 5: Define Routes**

- Open `config/routes.rb`.
- Add:

  ```ruby
  Rails.application.routes.draw do
    resources :photos
  end
  ```

- This automatically creates routes for all CRUD actions.

### **Step 6: Implement Controller Actions**

1. **Index Action (Read All)**

   - In `app/controllers/photos_controller.rb`, add:

     ```ruby
     def index
       photos = Photo.all
       render json: photos
     end
     ```

2. **Show Action (Read One)**

   ```ruby
   def show
     photo = Photo.find(params[:id])
     render json: photo
   end
   ```

3. **Create Action (Create)**

   ```ruby
   def create
     photo = Photo.new(photo_params)
     if photo.save
       render json: photo, status: :created
     else
       render json: photo.errors, status: :unprocessable_entity
     end
   end
   ```

4. **Update Action (Update)**

   ```ruby
   def update
     photo = Photo.find(params[:id])
     if photo.update(photo_params)
       render json: photo
     else
       render json: photo.errors, status: :unprocessable_entity
     end
   end
   ```

5. **Destroy Action (Delete)**

   ```ruby
   def destroy
     photo = Photo.find(params[:id])
     photo.destroy
     head :no_content
   end
   ```

6. **Strong Parameters**

   ```ruby
   private

   def photo_params
     params.require(:photo).permit(:name, :width, :height)
   end
   ```

### **Step 7: Test Your API**

1. **Start the Server**

   ```bash
   rails server
   ```

2. **Use Postman or Similar Tool**

   - **GET /photos**: Retrieve all photos.
   - **GET /photos/:id**: Retrieve a specific photo.
   - **POST /photos**: Create a new photo.
     - Body parameters:

       ```json
       {
         "photo": {
           "name": "Ocean",
           "width": 1024,
           "height": 768
         }
       }
       ```

   - **PUT /photos/:id**: Update an existing photo.
     - Body parameters similar to POST.
   - **DELETE /photos/:id**: Delete a photo.

### **Conclusion**

You've built a full set of CRUD operations for a `Photo` resource in a Rails API. This forms the backbone of many web applications.

---
