### **Installing Rails (API Mode) on Windows Using WSL**

**Prerequisites:**

- WSL and Ubuntu installed on your Windows machine.
- Ruby installed via `rbenv` (see separate guide for setup).

---

### **Step 1: Install Rails**

1. **Open Windows Terminal in WSL:**
   - Open your Windows Terminal terminal by searching for **Windows Terminal** in the Start Menu and then type `wsl` in **Command Prompt** or **Windows PowerShell**.

2. **Install Rails:**
   - Once Ruby is installed, you can install Rails:
     ```bash
     gem install rails
     ```

3. **Verify Rails Installation:**
   - Check that Rails was installed successfully by running:
     ```bash
     rails -v
     ```
   - You should see something like `Rails 7.x.x`.

---

### **Step 2: Install PostgreSQL and Development Libraries**

Since many Rails applications use PostgreSQL as the database, it’s essential to install PostgreSQL and the necessary development libraries.

1. **Install PostgreSQL Client and Development Libraries:**
   - Run the following commands in your WSL terminal:
     ```bash
     sudo apt update
     sudo apt install postgresql postgresql-contrib libpq-dev
     ```

   - The `libpq-dev` package is required to install the `pg` gem, which allows Rails to connect to PostgreSQL.

2. **Start PostgreSQL Service:**
   - Start the PostgreSQL service using:
     ```bash
     sudo service postgresql start
     ```

3. **Create a PostgreSQL User:**
   - Create a PostgreSQL superuser that matches your WSL username (e.g., `leon`). Run the following command:
     ```bash
     sudo -u postgres createuser -s your_username
     ```

4. **Create a PostgreSQL Database (Optional):**
   - If you want to create a database for testing, you can run:
     ```bash
     createdb my_database
     ```

---

### **Step 3: Create a New Rails API Application**

1. **Create a directory for your projects (optional):**
   - You can create a directory to store your Rails projects:
     ```bash
     mkdir ~/projects
     cd ~/projects
     ```

2. **Create a new Rails API app with PostgreSQL:**
   - Run the following command to create a new Rails API application using PostgreSQL as the database:
     ```bash
     rails new my_api --api --database=postgresql
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

---

### **Step 4: Start the Rails Server**

1. **Create the Database:**
   - Run the following command to create the development and test databases:
     ```bash
     rails db:create
     ```

2. **Run Migrations:**
   - If there are migrations to run, execute:
     ```bash
     rails db:migrate
     ```

3. **Start the Rails server:**
   - Start the Rails server with:
     ```bash
     rails server
     ```

4. **Access Your Rails API:**
   - Open your browser and go to `http://localhost:3000`. Your Rails API should now be running.

---

### **Additional Notes:**

- **No Need for Node.js:** Since we're using Rails in API mode, **Node.js is not required** for this setup.
- **PostgreSQL Service:** Ensure PostgreSQL is running when you work on your Rails app by starting it with `sudo service postgresql start` if necessary.
- **Use WSL and Ubuntu for Rails Development:** WSL gives you a full Linux environment on Windows, making Rails development smoother, especially when dealing with Ruby and gem dependencies.

---

**Rails (in API mode) is now installed and running on your Windows computer using WSL with PostgreSQL as the database!**
