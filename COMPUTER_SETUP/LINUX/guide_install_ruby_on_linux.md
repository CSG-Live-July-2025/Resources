#### **Installing Ruby on Linux**

**Prerequisites:**

- A Linux computer (instructions are for Ubuntu/Debian distributions).
- Internet access.

**Step 1: Update Package Lists**

1. **Open Terminal.**

2. **Update package lists:**

   ```bash
   sudo apt update
   ```

**Step 2: Install Dependencies**

1. **Install required packages:**

   ```bash
   sudo apt install -y build-essential libssl-dev libreadline-dev zlib1g-dev
   ```

**Step 3: Install rbenv and ruby-build**

1. **Install rbenv:**

   ```bash
   git clone https://github.com/rbenv/rbenv.git ~/.rbenv
   echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
   echo 'eval "$(rbenv init -)"' >> ~/.bashrc
   source ~/.bashrc
   ```

2. **Install ruby-build:**

   ```bash
   git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
   ```

**Step 4: Install Ruby**

1. **List available Ruby versions:**

   ```bash
   rbenv install -l
   ```

2. **Install the desired Ruby version (e.g., 3.1.2):**

   ```bash
   rbenv install 3.1.2
   rbenv global 3.1.2
   ```

3. **Verify Ruby Installation:**

   ```bash
   ruby -v
   ```

   - You should see something like `ruby 3.1.2p0`.

**Ruby is now installed on your Linux machine!**

---
