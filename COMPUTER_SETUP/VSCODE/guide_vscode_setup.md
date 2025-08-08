### **Introduction**

**What is VSCode?**

Visual Studio Code (VSCode) is a free, open-source code editor developed by Microsoft. It's popular for its versatility, user-friendly interface, and extensive extension ecosystem.

### **Why Use VSCode?**

- **Intuitive Interface:** Easy to navigate for beginners.
- **Customization:** Tailor the editor to your needs with themes and settings.
- **Extensions:** Enhance functionality with plugins for different languages and tools.

---

### **Step 1: Installing VSCode**

1. **Download VSCode**

   - Go to the [VSCode website](https://code.visualstudio.com/download).
   - Choose the version for your operating system (**macOS** or **Windows**).

2. **Install VSCode**

   - **macOS**: 
     - Open the `.dmg` file and drag the VSCode icon into the Applications folder.
   - **Windows**: 
     - Run the installer (`.exe`) and follow the prompts.
   - Accept the terms and choose the default options.

---

### **Step 2: Launching VSCode**

- **macOS**: Open VSCode from your **Applications** folder or search for it via **Spotlight**.
- **Windows**: Open VSCode from your **Start Menu** or desktop shortcut.

Familiarize yourself with the interface:
  - **Explorer:** View and manage your files and folders.
  - **Editor Area:** Where you write your code.
  - **Terminal:** Access a command-line interface within VSCode.

---

### **Step 3: Set Up the `code .` Command in Your Terminal**

#### **For macOS:**

1. **Open Command Palette:**
   - Press `Cmd + Shift + P`.

2. **Install 'code' Command in PATH:**
   - In the Command Palette, type **"Shell Command: Install 'code' command in PATH"** and select it.
   - This step enables the `code` command to be used from your terminal.

3. **Verify the Command:**
   - Open your terminal and run the following command:
     ```bash
     code .
     ```
   - This will open the current folder in VSCode.

#### **For Windows (WSL)**:

1. **Add the Microsoft GPG Key and Repository:**
   - Open the **Ubuntu terminal** in WSL and run:
     ```bash
     sudo apt update
     sudo apt install software-properties-common apt-transport-https wget
     wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
     sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
     ```

2. **Install VSCode:**
   - Run the following command in the terminal:
     ```bash
     sudo apt update
     sudo apt install code
     ```

3. **Verify the Command:**
   - Close VSCode if it's open, then open the **Ubuntu terminal** and run:
     ```bash
     code .
     ```
   - This will open the current directory in VSCode.

   > **If prompted to install WSL version:** If running `code .` asks you y/n to install the WSL version of VSCode, select **"No"** and then run:
   > ```bash
   > sudo apt remove -y code && sudo apt autoremove -y
   > ```
   > After this, try running `code .` again and it should work properly with your Windows VSCode installation.

4. **Close and Reopen VSCode:**
   - After installation, close VSCode completely, then reopen it. The `code .` command should now work properly in your **Ubuntu (WSL)** terminal.

---

### **Step 4: Set Up the `open .` Command Using `explorer.exe` (For Windows)**

#### **For WSL Users on Windows**:

1. **Create an Alias for `open .` Using `explorer.exe`:**

   1. Open your terminal in WSL.

   2. Edit your `.zshrc` (or `.bashrc`) file using `nano`:
      ```bash
      nano ~/.zshrc
      ```
      (If you’re using bash, run `nano ~/.bashrc` instead.)

   3. Add the following line to create an alias for `open`:
      ```bash
      alias open='explorer.exe'
      ```

   4. Save and exit **nano**:
      - Press `Ctrl + X` to exit.
      - Press `Y` to confirm saving, and hit **Enter** to confirm the file name.

   5. Apply the changes by running:
      ```bash
      source ~/.zshrc
      ```
      (If you’re using bash, run `source ~/.bashrc` instead.)

2. **Use the `open .` Command:**
   - Now you can use the `open .` command in WSL, and it will open the current directory in Windows File Explorer:
     ```bash
     open .
     ```

---

### **Step 5: Customizing VSCode**

1. **Changing the Theme**

   - Go to `Code > Preferences > Color Theme` (macOS) or `File > Preferences > Color Theme` (Windows).
   - Choose a theme you like (e.g., Dark+, Light+).

2. **Adjusting Settings with Command Palette**

   - **Open Command Palette:**
     - **macOS**: Press `Cmd + Shift + P`
     - **Windows**: Press `Ctrl + Shift + P`
   - **Open User Settings (JSON):**
     - In the Command Palette, type **"Preferences: Open User Settings (JSON)"** and select it.
   - **Add Custom Settings:**
     - Paste the following into the `settings.json` file to customize your settings:
       ```json
       {
           "editor.tabSize": 2,
           "files.autoSave": "onFocusChange",
           "editor.parameterHints.enabled": false,
           "explorer.openEditors.visible": 0,
           "workbench.editor.closeEmptyGroups": false,
           "security.workspace.trust.untrustedFiles": "open",
           "editor.accessibilitySupport": "off",
           "editor.wordWrap": "on",
           "diffEditor.ignoreTrimWhitespace": false,
           "window.zoomLevel": 2
       }
       ```
   - **Save the file:** 
     - **macOS**: Press `Cmd + S` to save.
     - **Windows**: Press `Ctrl + S` to save.

---

### **Step 6: Installing Extensions**

**What are Extensions?**

Extensions add features to VSCode, such as support for additional languages or tools.

1. **Access the Extensions Marketplace**

   - Click the Extensions icon on the left sidebar (looks like four squares).

2. **Install Recommended Extensions**

   - **Prettier – Code Formatter:**
     - Formats your code automatically.
   - **ESLint:**
     - Helps identify and fix problems in your JavaScript code.

3. **Search and Install**

   - Use the search bar to find an extension.
   - Click **Install** to add it to VSCode.

---

### **Step 7: Using the Integrated Terminal**

- **macOS**: 
  - Open the terminal by going to `View > Terminal` or press `` Cmd + ` ``.
- **Windows**: 
  - Open the terminal by going to `View > Terminal` or press `` Ctrl + ` ``.
  
- **For WSL Users on Windows**: 
  - If you’re using **WSL**, you can switch the terminal to **Ubuntu** by typing:
    ```bash
    wsl
    ```
    This will open the **Ubuntu** shell, where you can run Linux commands.

- **Run Commands:**
  - You can execute Git commands, run your code, and more.

---

### **Step 8: Keyboard Shortcuts**

- **Common Shortcuts:**
  - **Open Command Palette:**
    - **macOS**: Press `Cmd + Shift + P`
    - **Windows**: Press `Ctrl + Shift + P`
  - **Toggle Terminal:**
    - **macOS**: Press `` Cmd + ` ``
    - **Windows**: Press `` Ctrl + ` ``
  - **Save File:**
    - **macOS**: Press `Cmd + S`
    - **Windows**: Press `Ctrl + S`
  - **Find in File:**
    - **macOS**: Press `Cmd + F`
    - **Windows**: Press `Ctrl + F`

---

### **Step 9: Working with Projects**

1. **Open a Folder**

   - **macOS**: Go to `Code > Open Folder` and navigate to your project's directory.
   - **Windows**: Go to `File > Open Folder` and navigate to your project's directory.

2. **Creating Files**

   - Right-click in the Explorer pane and select `New File`.
   - Name your file with the appropriate extension (e.g., `index.html`, `app.js`).

---

### **Conclusion**

With VSCode set up and customized, you're ready to start coding! Explore additional features and extensions to enhance your development experience.
