# **Installing Ruby on Windows via WSL (Windows Subsystem for Linux) with Ubuntu and rbenv**

**Prerequisites:**

- A Windows computer running Windows 10 or later.
- Internet access.

---

### **Step 1: Install Homebrew on Ubuntu (via WSL)**

1. **Install Prerequisites:**
   - Run the following commands:
     ```bash
     sudo apt update && sudo apt upgrade -y
     sudo apt install -y build-essential procps curl file git
     ```

2. **Install Homebrew:**
   - Run:
     ```bash
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```

3. **Add Homebrew to PATH:**
   - Run:
     ```bash
     echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.zshrc
     source ~/.zshrc
     ```

4. **Verify Homebrew Installation:**
   - Run:
     ```bash
     brew --version
     ```

5. **Purpose of Homebrew:**
   - Homebrew is a package manager that makes it easy to install software and dependencies like Node.js or PostgreSQL. It simplifies managing tools you’ll need during the course.

---

### **Step 2: Install rbenv and ruby-build**

#### **What is rbenv and Why Are We Using It?**
rbenv is a Ruby version manager that allows you to install and manage multiple versions of Ruby on your system. It ensures that you can easily switch between versions for different projects and keeps your system’s Ruby environment organized and consistent.

1. **Install rbenv:**
   - Run:
     ```bash
     brew install rbenv ruby-build
     ```

2. **Add rbenv to Your PATH:**
   - Ensure the following lines are in your `.zshrc` file:
     ```bash
     export PATH="$HOME/.rbenv/bin:$PATH"
     eval "$(rbenv init - zsh)"
     ```
   - Apply changes:
     ```bash
     source ~/.zshrc
     ```

3. **Verify rbenv Installation:**
   - Run:
     ```bash
     rbenv --version
     ```

---

### **Step 3: Install Ruby 3.3.4**

1. **Install Ruby:**
   - Run:
     ```bash
     rbenv install 3.3.4
     ```

2. **Set Ruby Version Globally:**
   - Run:
     ```bash
     rbenv global 3.3.4
     ```

3. **Verify Ruby Installation:**
   - Check the installed Ruby version:
     ```bash
     ruby -v
     ```
   - The output should be `ruby 3.3.4`.

---

### **Step 4: Use the Setup Script**

To automate the Ruby installation process, use this script:

#### **What Does the Script Do?**
This script automates the installation of Homebrew, `rbenv`, and Ruby. It ensures all dependencies are installed and Ruby is configured correctly for your system.

#### **Where to Put the Script:**
1. Save the script file to your Linux home directory (e.g., `/home/<username>/setup_ruby.sh`).
2. Open the terminal in the directory where the script is saved.

#### **How to Use the Script:**
1. Make the script executable by running:
   ```bash
   chmod +x setup_ruby.sh
   ```
2. Execute the script:
   ```bash
   ./setup_ruby.sh
   ```
3. Follow the output messages to confirm everything is installed successfully.

#### **The Script:**
```bash
#!/bin/bash

# Update and upgrade system
sudo apt update && sudo apt upgrade -y

# Install Homebrew
sudo apt install -y build-essential procps curl file git
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc

# Install rbenv and ruby-build
brew install rbenv ruby-build
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init - zsh)"

# Install Ruby
rbenv install 3.3.4
rbenv global 3.3.4

# Verify Ruby installation
ruby -v

echo "Ruby setup complete!"
```

---

### **Ruby is Now Installed on WSL!**
You’re ready to start developing Ruby and Rails projects on your Windows machine.

---

### **Encourage Questions**
If you run into any issues during setup, don’t hesitate to reach out to your instructor or teaching assistant for help!
