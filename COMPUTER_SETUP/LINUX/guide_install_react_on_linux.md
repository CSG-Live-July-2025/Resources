#### **Installing Node.js and React.js on Linux**

**Prerequisites:**

- Linux computer (instructions are for Ubuntu/Debian distributions).

**Step 1: Install Node.js and npm**

1. **Update package lists:**

   ```bash
   sudo apt update
   ```

2. **Install Node.js and npm:**

   ```bash
   sudo apt install -y nodejs npm
   ```

3. **Verify Installation:**

   ```bash
   node -v
   npm -v
   ```

   - You should see version numbers for both.

**Step 2: Create a New React Application**

1. **Navigate to your projects directory (optional):**

   ```bash
   cd ~/projects
   ```

2. **Create a new React app using Create React App:**

   ```bash
   npx create-react-app my-app
   ```

3. **Navigate into your new app's directory:**

   ```bash
   cd my-app
   ```

4. **Start the development server:**

   ```bash
   npm start
   ```

   - Your app should now be running on `http://localhost:3000`.

**React.js is now installed and running on your Linux machine!**

---

### **Additional Notes**

- **Using rbenv to Manage Ruby Versions:**

  - **macOS and Linux:**

    - We used `rbenv` to install and manage Ruby versions, ensuring you have the specific version needed for your projects.

- **Using nvm (Node Version Manager) for Node.js:**

  - **macOS and Linux:**

    - If you prefer managing multiple Node.js versions, you can install `nvm`.

    - **Install nvm:**

      ```bash
      curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
      source ~/.bashrc  # or source ~/.zshrc for Zsh
      ```

    - **Install a specific Node.js version:**

      ```bash
      nvm install 18.16.0
      nvm use 18.16.0
      ```

    - **Verify Node.js version:**

      ```bash
      node -v
      ```

- **Windows Subsystem for Linux (WSL):**

  - For a Unix-like environment on Windows, you can use WSL.

  - **Install WSL:**

    - Open PowerShell as Administrator and run:

      ```bash
      wsl --install
      ```

    - Restart your computer.

    - By default, Ubuntu will be installed.

  - **Proceed with Linux installation steps within WSL.**

- **Avoiding Permission Issues with npm on Linux and macOS:**

  - Configure npm to install global packages without `sudo`:

    ```bash
    mkdir "${HOME}/.npm-packages"
    npm config set prefix "${HOME}/.npm-packages"
    echo 'export PATH="$HOME/.npm-packages/bin:$PATH"' >> ~/.bashrc  # or ~/.zshrc
    source ~/.bashrc  # or source ~/.zshrc
    ```

---

