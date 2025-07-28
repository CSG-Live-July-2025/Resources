## Installing Node.js and React.js on macOS

### Prerequisites

- [Homebrew](https://brew.sh/) installed on your Mac.

---

### Step 1: Install Node.js

1. **Install Node.js using Homebrew:**

   ```bash
   brew install node
   ```

2. **Verify Node.js and npm Installation:**

   ```bash
   node -v
   npm -v
   ```
   - You should see version numbers for both. If Node.js and npm are installed correctly, you’re good to go.

---

### Step 2: Create a New React Application

React can be set up in multiple ways. Below are **two popular approaches**:

#### Option A: Using Vite (Recommended for Faster Development)

1. **Navigate to your projects directory (optional):**

   ```bash
   cd ~/projects
   ```

2. **Create a new React app using Vite:**

   ```bash
   # Using npm
   npm create vite@latest my-app -- --template react

   # If you use yarn, you can do:
   # yarn create vite my-app --template react
   ```
   - This initializes a minimal React project with Vite’s fast development server.

3. **Navigate into your new app’s directory and install dependencies (if prompted):**

   ```bash
   cd my-app
   npm install
   ```

4. **Start the development server:**

   ```bash
   npm run dev
   ```
   - Your app should now be running at `http://localhost:5173` (Vite’s default port).

**Why Vite?**  
- **Faster startup and rebuild times** compared to Webpack.  
- **Simple configuration** via a `vite.config.js` file.  
- **Modern ES modules** approach that speeds up development.

---

#### Option B: Using Create React App (CRA)

If you’d rather use **Create React App**, here’s how:

1. **Navigate to your projects directory (optional):**

   ```bash
   cd ~/projects
   ```

2. **Create a new React app using CRA:**

   ```bash
   npx create-react-app my-app
   ```

3. **Navigate into your new app’s directory:**

   ```bash
   cd my-app
   ```

4. **Start the development server:**

   ```bash
   npm start
   ```
   - Your app should now be running on `http://localhost:3000`.

**Why CRA?**  
- **Long-standing default** in the React community.  
- **Webpack-based** with a lot of tooling already configured.  
- **Plenty of tutorials** still reference CRA.

---

### What’s the Difference?

- **Vite**: A modern build tool offering fast dev server startup and lighter configuration. Recommended for new projects that value speed and simplicity.  
- **Create React App (CRA)**: The older default. Uses Webpack behind the scenes, which can be **slower** to start/rebuild in large projects, but it’s still widely used and well-documented.

---

### Next Steps

1. **Open Your Project in a Code Editor**  
   - We recommend [Visual Studio Code](https://code.visualstudio.com/) or [Atom](https://atom.io/).  

2. **Explore Your Folder Structure**  
   - `src/` contains most of your app’s code (components, hooks, etc.).  
   - `public/` (in CRA) or `index.html` in root (with Vite) is where your app’s HTML is served.  

3. **Start Coding!**  
   - Edit `App.jsx` (Vite) or `App.js` (CRA) to customize your React application.  
   - Enjoy **hot module replacement**: changes appear instantly in your browser.

---

## React.js is Now Installed and Running on Your Mac!

Whether you chose **Vite** or **CRA**, you can now develop React applications locally. If you’re new, don’t hesitate to try them both—understanding their differences will help you pick the right tool for future projects.
