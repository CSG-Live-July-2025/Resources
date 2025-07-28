# Windows (WSL) Terminal Setup Guide

### **Step 1: Install WSL (Windows Subsystem for Linux)**

#### **What is WSL and Why Are We Using It?**
WSL (Windows Subsystem for Linux) is a compatibility layer that allows you to run a full Linux environment directly on Windows. It’s lightweight, doesn’t require a virtual machine, and provides access to powerful Linux tools and software.

We’re using WSL because Linux is widely used in software development, and many tools (like Ruby on Rails) are optimized for Linux environments. Using WSL ensures consistency across different operating systems and helps avoid compatibility issues.

#### **Why Are WSL’s Files and Folders in a Different Place?**
WSL runs in its own virtualized environment, separate from the Windows file system. Files and folders created in WSL are stored in its Linux file system, located in `/home/<username>`. While you can access Windows files from WSL via `/mnt`, working directly in the WSL Linux file system (e.g., `/home/<username>/projects`) provides better performance and avoids issues with file permissions and compatibility.

---

1. **Open PowerShell as Administrator:**
   - Right-click the Start menu and select **Windows PowerShell (Admin)**.

2. **Install WSL:**
   - Run the following command in the PowerShell window:
     ```powershell
     wsl --install
     ```
   - This installs WSL 2 and the default Linux distribution (Ubuntu). If Ubuntu is already installed, the command skips reinstallation.

3. **Restart Your Computer:**
   - If prompted, restart your computer to complete the installation.

4. **Verify Installation:**
   - Open PowerShell and run:
     ```powershell
     wsl --list --verbose
     ```
   - Ensure `VERSION` shows `2` for the installed distribution (e.g., Ubuntu). If not, set it to WSL2 with:
     ```powershell
     wsl --set-version Ubuntu 2
     ```

5. **Check System Resources for WSL2:**
   - Verify that virtualization is enabled, as WSL2 requires it. If the command above gives an error, follow these steps:
     - Restart your computer and enter the BIOS/UEFI settings (this typically involves pressing a key like `Esc`, `Del`, or `F2` during boot).
     - Look for a setting related to **virtualization** (e.g., Intel VT-x or AMD-V) and ensure it is enabled.
     - Save changes and reboot.
   - **Common Issues:**
     - If you’re unable to find the virtualization settings, check your computer's manual or search online for your specific motherboard model.
     - Older systems may not support WSL2; in that case, WSL1 is an alternative, though it has reduced compatibility with certain development tools.

---

### **Step 2: Set Up Ubuntu**

1. **Launch Ubuntu:**
   - Open the Ubuntu app from the Start menu.

2. **Create a Linux User:**
   - Follow the prompts to create a username and password for your Linux environment.

3. **Update and Upgrade Ubuntu:**
   - Run the following commands to update and upgrade packages:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

---

### **Step 3: Install Zsh and Set as Default Shell**

#### **What Are Zsh and Bash?**
- **Bash** (Bourne Again Shell) is the default command-line interpreter for many Linux distributions. It allows users to interact with the operating system by running commands, scripts, and programs.
- **Zsh** (Z Shell) is another shell, similar to Bash, but with additional features like advanced autocompletion, plugins, and themes. It’s highly customizable and provides a more modern and user-friendly experience.

#### **Why Zsh Over Bash?**
Zsh offers a more powerful and user-friendly experience compared to Bash, making it easier to navigate and customize your terminal environment. Features like advanced autocompletion, plugins, and themes make it a popular choice for developers.

1. **Install Zsh:**
   - Run:
     ```bash
     sudo apt install zsh -y
     ```

2. **Set Zsh as Default Shell:**
   - Run:
     ```bash
     chsh -s $(which zsh)
     ```
   - Log out and back into your Ubuntu terminal for the changes to take effect.

3. **Install Oh-My-Zsh (Optional):**
   - Run:
     ```bash
     sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
     ```
   - This adds themes and plugins to enhance your Zsh experience.

4. **Configure Colors, Autocompletion, and Aliases:**
   - Run the following commands:
     ```bash
     echo "export CLICOLOR=1" >> ~/.zshrc
     echo "export LSCOLORS=GxFxCxDxBxegedabagaced" >> ~/.zshrc
     echo "export EDITOR='code -w'" >> ~/.zshrc
     echo "" >> ~/.zshrc
     echo "alias ls='ls --color=auto -h'" >> ~/.zshrc
     echo "" >> ~/.zshrc
     echo "PROMPT='%F{cyan}%~%f %# '" >> ~/.zshrc
     echo "autoload -Uz compinit && compinit" >> ~/.zshrc
     echo "zstyle ':completion:*' matcher-list '' 'm:{a-zA-Z}={A-Za-z}'" >> ~/.zshrc
     echo "zstyle ':completion:*' menu select" >> ~/.zshrc
     echo "zstyle ':completion:*' completer _expand_alias _complete _ignored" >> ~/.zshrc
     echo "gem: --no-document" >> ~/.gemrc
     ```
   - Restart the terminal or apply the changes:
     ```bash
     source ~/.zshrc
     ```

---

### **Step 4: Accessing and Understanding Windows Files in WSL**

1. **Accessing Windows Files:**
   - In WSL, you can access your Windows files under the `/mnt` directory. For example:
     ```bash
     cd /mnt/c/Users/<YourWindowsUsername>/Desktop
     ```
   - Replace `<YourWindowsUsername>` with your actual Windows username.

2. **Creating a Shortcut for Faster Access:**
   - To avoid typing the full path every time, you can create a symbolic link:
     ```bash
     ln -s /mnt/c/Users/<YourWindowsUsername>/Desktop ~/desktop
     ```
   - This creates a shortcut to your Windows Desktop folder, accessible by running:
     ```bash
     cd ~/desktop
     ```

3. **Why You Shouldn’t Edit or Create Files in Windows Directories via WSL:**
   - Editing or creating files directly in `/mnt` (your Windows directories) while in WSL can lead to performance issues and file permission conflicts. The WSL environment is optimized for files stored in its native Linux file system (e.g., `/home/<username>/projects`).
   - **Best Practice:** Store and edit your project files within the Linux file system (e.g., `/home/<username>/projects`) for better compatibility and performance.

---

### **Step 5: Install the WSL Extension in Visual Studio Code**

1. **Why Use the WSL Extension?**
   - The **Remote - WSL** extension in VS Code allows you to work seamlessly with your WSL environment, enabling you to open files and projects directly from WSL in VS Code.

2. **Install Visual Studio Code:**
   - Download and install Visual Studio Code from [https://code.visualstudio.com/](https://code.visualstudio.com/).

3. **Install the Remote - WSL Extension:**
   - Open Visual Studio Code.
   - Go to the Extensions view by pressing `Ctrl+Shift+X`.
   - Search for **"Remote - WSL"** and click **Install**.

4. **Open WSL in VS Code:**
   - Launch WSL and navigate to your project directory (e.g., `/home/<username>/projects`).
   - Open the project in VS Code by typing:
     ```bash
     code .
     ```
   - This will open the current directory in VS Code, connected to your WSL environment.

5. **Verify the WSL Integration:**
   - The title bar in VS Code should show `[WSL: Ubuntu]`, confirming it’s connected to your WSL instance.

---

### **Step 6: Use the Setup Script**

To simplify the terminal setup process, use the following script:

#### **What Does the Script Do?**
This script automates the installation of essential tools, Zsh, and Oh-My-Zsh, and configures your terminal for colors, autocompletion, and aliases. It saves time and ensures consistency in your setup.

#### **Where to Put the Script:**
1. Save the script file to your Linux home directory (e.g., `/home/<username>/setup_terminal.sh`).
2. Open the terminal in the directory where the script is saved.

#### **How to Use the Script:**
1. Make the script executable by running:
   ```bash
   chmod +x setup_terminal.sh
   ```
2. Execute the script:
   ```bash
   ./setup_terminal.sh
   ```
3. Follow any on-screen prompts to confirm the installation.

#### **The Script:**
```bash
#!/bin/bash

# Update and upgrade system
sudo apt update && sudo apt upgrade -y

# Install basic tools and dependencies
sudo apt install -y build-essential curl git zsh

# Install Zsh and set it as the default shell
sudo apt install zsh -y
chsh -s $(which zsh)

# Install Oh-My-Zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Configure Colors, Autocompletion, and Aliases
cat <<EOL >> ~/.zshrc
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced
export EDITOR='code -w'
alias ls='ls --color=auto -h'
PROMPT='%F{cyan}%~%f %# '
autoload -Uz compinit && compinit
zstyle ':completion:*' matcher-list '' 'm:{a-zA-Z}={A-Za-z}'
zstyle ':completion:*' menu select
zstyle ':completion:*' completer _expand_alias _complete _ignored
EOL

# Apply changes
source ~/.zshrc

echo "Terminal setup complete! Restart your terminal to apply all changes."
```

---

### **Post-Setup Tests**

After running the script, use these commands to confirm your terminal is set up correctly:

1. Check if Zsh is the default shell:
   ```bash
   echo $SHELL
   ```
   - The output should be `/usr/bin/zsh`.

2. Test if aliases and colors are working:
   ```bash
   ls --color=auto
   ```
   - You should see color-coded output for directories and files.

---

### **Encourage Questions**
If you run into any issues during setup, don’t hesitate to reach out to your instructor or teaching assistant for help!

