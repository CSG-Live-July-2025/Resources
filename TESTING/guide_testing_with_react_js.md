## **Installing Node.js and React.js on Windows Using WSL**

### **Prerequisites**

- **WSL** and **Ubuntu** installed on your Windows machine.  
- **Internet access**.

---

### **Step 1: Install Node.js and npm Using WSL**

1. **Open Windows Terminal in WSL**  
   - Open your Windows Terminal by searching for **Windows Terminal** in the Start Menu, then type `wsl` in **Command Prompt** or **Windows PowerShell** to enter WSL (Ubuntu).

2. **Update Ubuntu’s package list**  
   - Ensure your system is up to date:
     ```bash
     sudo apt update
     sudo apt upgrade
     ```

3. **Install Node.js and npm**  
   - Install the latest **LTS** version of Node.js along with **npm** (Node Package Manager) using the NodeSource repository:
     ```bash
     curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
     sudo apt install -y nodejs
     ```

4. **Verify Node.js and npm Installation**  
   - Check the version numbers to confirm successful installation:
     ```bash
     node -v
     npm -v
     ```
   - You should see version numbers for both.

---

### **Step 2: Create a New React Application Using Vite**

1. **Navigate to your projects directory (optional)**  
   - If you prefer a specific folder structure, you can create and move into it:
     ```bash
     mkdir ~/projects
     cd ~/projects
     ```

2. **Create a new React app using Vite**  
   - Use `npm create vite@latest` (which comes with npm 6+). This will prompt you for your project name and framework/template:
     ```bash
     npm create vite@latest my-app -- --template react
     ```
   - Alternatively (if not prompted), you can specify everything in one command:
     ```bash
     npm create vite@latest my-app -- --template react
     ```

3. **Navigate into your new app’s directory and install dependencies**  
   ```bash
   cd my-app
   npm install
   ```

4. **Start the development server**  
   - Start the Vite development server:
     ```bash
     npm run dev
     ```
   - By default, your React app will be served at `http://localhost:5173`.

5. **Access your React application**  
   - In your Windows browser, go to `http://localhost:5173` to see your React app running.  
   - If you don’t see anything, make sure the server is running in WSL and that you haven’t blocked network access.

---

### **Additional Notes**

- **Using WSL for Node.js and React**: By running Node.js and Vite inside Ubuntu on WSL, you can develop in a Linux-like environment on Windows, which is especially beneficial if your workflow requires Unix-based tools.  
- **No Need for Windows Node.js Installer**: Because you’re running Node.js inside Ubuntu on WSL, there’s no need to install the Windows version. Everything runs seamlessly in the WSL environment.  
- **Why Vite Over CRA**: Vite offers a faster startup and simpler configuration than the traditional Create React App (CRA). If you still want CRA for any reason, you can install it via:
  ```bash
  npx create-react-app my-app
  ```
  but most newer projects benefit from Vite’s performance and modern tooling.

---

**Node.js and a Vite-based React.js application are now installed and running on your Windows computer using WSL!**
