### **Installing Rails (API Mode) on macOS**

**Prerequisites:**

- Ruby installed.

---

### **Step 1: Install Rails**

1. **Install Rails gem:**

   ```bash
   gem install rails
   ```

2. **Verify Rails Installation:**

   ```bash
   rails -v
   ```

   - You should see something like `Rails 7.x.x`.

---

### **Step 2: Install PostgreSQL**

PostgreSQL is a popular database choice for Rails applications. Follow these steps to install and set it up.

1. **Install PostgreSQL using Homebrew:**

   ```bash
   brew install postgresql
   ```

2. **Start PostgreSQL Service:**
   - Start PostgreSQL and set it to launch at login:
     ```bash
     brew services start postgresql
     ```

3. **Verify PostgreSQL Installation:**
   - To check if PostgreSQL is running, use:
     ```bash
     psql --version
     ```
   - You should see the PostgreSQL version output, confirming it’s installed.

### **Step 3: Create a New Rails API Application Using PostgreSQL**

1. **Create a directory for your projects (optional):**

   ```bash
   mkdir ~/projects
   cd ~/projects
   ```

2. **Create a new Rails API app with PostgreSQL as the database:**

   ```bash
   rails new my_api --api --database=postgresql
   ```

   - The `--api` flag creates a Rails application optimized for API-only use.
   - The `--database=postgresql` flag configures the application to use PostgreSQL as the database.

3. **Navigate into your new app's directory:**

   ```bash
   cd my_api
   ```

---

### **Step 4: Initialize Git Repository (Optional but Recommended)**

1. **Initialize Git:**

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

---

### **Step 5: Start the Rails Server**

1. **Create the Database:**

   ```bash
   rails db:create
   ```

2. **Run Migrations:**

   ```bash
   rails db:migrate
   ```

3. **Start the Rails Server:**

   ```bash
   rails server
   ```

4. **Access Your Rails API:**
   - Open your browser and go to `http://localhost:3000`. Your Rails API should now be running.

---

### **Additional Notes:**

- **No Need for Node.js:** Since we're using Rails in API mode, **Node.js is not required**.
- **PostgreSQL Service:** The PostgreSQL service will continue running in the background, so you can access it any time you work on your Rails app.

---

**Rails (in API mode) is now installed and running on your Mac using PostgreSQL as the database!**
