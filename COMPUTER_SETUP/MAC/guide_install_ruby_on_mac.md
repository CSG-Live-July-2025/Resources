#### **Installing Ruby on macOS**

**Prerequisites:**

- A Mac computer running macOS.
- Internet access.

**Step 1: Install Homebrew**

Homebrew is a package manager for macOS that simplifies the installation of software.

1. **Open Terminal:**

   - Find Terminal in `Applications > Utilities > Terminal`.

2. **Install Homebrew:**

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

   - Follow the on-screen instructions. You may need to enter your password.

3. **Verify Homebrew Installation:**

   ```bash
   brew -v
   ```

   - You should see the Homebrew version installed.

**Step 2: Install Ruby**

1. **Install rbenv and ruby-build:**

   ```bash
   brew install rbenv ruby-build
   ```

2. **Set up rbenv in your shell:**

   - For **Zsh** (default in newer macOS versions):

     ```bash
     echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
     source ~/.zshrc
     ```

   - For **Bash**:

     ```bash
     echo 'eval "$(rbenv init - bash)"' >> ~/.bash_profile
     source ~/.bash_profile
     ```

3. **Install Ruby using rbenv:**

   - List available Ruby versions:

     ```bash
     rbenv install -l
     ```

   - Install the desired Ruby version (e.g., 3.3.4):

     ```bash
     rbenv install 3.3.4
     rbenv global 3.3.4
     ```

4. **Verify Ruby Installation:**

   ```bash
   ruby -v
   ```

   - You should see something like `ruby 3.3.4`.

**Ruby is now installed on your Mac!**

---
