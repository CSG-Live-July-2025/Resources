### **Introduction**

**What is RuboCop?**

RuboCop is a code analyzer and formatter for Ruby. It helps you follow the Ruby style guide, ensuring your code is consistent and free of common errors.

### **Why Use RuboCop?**

- **Consistency:** Maintains a uniform coding style across your project.
- **Error Detection:** Identifies syntax errors and bad practices.
- **Learning Tool:** Teaches you idiomatic Ruby code patterns.

### **Prerequisites**

- Ruby installed on your computer.
- Basic understanding of Ruby and VSCode.

### **Step 1: Install Ruby**

- If you haven't installed Ruby, download it from the [Ruby website](https://www.ruby-lang.org/en/downloads/).
- Verify installation:

  ```bash
  ruby -v
  ```

### **Step 2: Install RuboCop Gem**

1. **Open Terminal**

2. **Install RuboCop**

   ```bash
   gem install rubocop
   ```

### **Step 3: Initialize RuboCop Configuration**

1. **Navigate to Your Project Directory**

   ```bash
   cd path/to/your/project
   ```

2. **Create a RuboCop Configuration File**

   ```bash
   rubocop --auto-gen-config
   ```

   - This generates a `.rubocop.yml` file with default settings.

### **Step 4: Install RuboCop Extension in VSCode**

1. **Open VSCode**

2. **Go to Extensions**

   - Click on the Extensions icon.

3. **Search for "ruby-rubocop"**

   - Install the extension **ruby-rubocop** by misogi.

### **Step 5: Configure VSCode to Use RuboCop**

1. **Open Settings**

   - Go to `File > Preferences > Settings`.

2. **Search for "Ruby RuboCop Path"**

   - Ensure the path to RuboCop is correct (usually it's set automatically).

3. **Enable Format on Save**

   - Search for "Format on Save" and enable it.

4. **Set RuboCop as Default Formatter**

   - In your project, create a `.vscode/settings.json` file.
   - Add:

     ```json
     {
       "ruby.format": "rubocop"
     }
     ```

### **Step 6: Test RuboCop**

1. **Create a Ruby File**

   - In your project, create `app.rb`.

2. **Write Some Code with Style Issues**

   ```ruby
   def say_hello
   puts "Hello, World!"
   end
   ```

3. **Observe RuboCop in Action**

   - RuboCop should underline issues.
   - Save the file to auto-correct formatting.

### **Conclusion**

You've set up RuboCop in VSCode to help you write clean and consistent Ruby code. It will guide you toward best practices as you develop.

---
