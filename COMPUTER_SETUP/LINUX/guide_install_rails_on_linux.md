#### **Installing Rails (API Mode) on Linux**

**Prerequisites:**

- Ruby installed.

**Step 1: Install Rails**

1. **Install Rails gem:**

   ```bash
   gem install rails
   ```

2. **Rehash rbenv shims:**

   ```bash
   rbenv rehash
   ```

3. **Verify Rails Installation:**

   ```bash
   rails -v
   ```

   - You should see something like `Rails 7.x.x`.

**Step 2: Create a New Rails API Application**

1. **Create a directory for your projects (optional):**

   ```bash
   mkdir ~/projects
   cd ~/projects
   ```

2. **Create a new Rails API app:**

   ```bash
   rails new my_api --api
   ```

3. **Navigate into your new app's directory:**

   ```bash
   cd my_api
   ```

4. **Initialize Git repository (optional but recommended):**

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

5. **Start the Rails server:**

   ```bash
   rails server
   ```

   - Your API is now running at `http://localhost:3000`.

**Rails (in API mode) is now installed and running on your Linux machine!**

**Note:** Since we're using Rails in API mode, **Node.js is not required** for the backend.

---
