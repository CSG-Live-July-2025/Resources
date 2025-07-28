Debugging in Rails**

**Prerequisites:**

- Basic understanding of Ruby and Rails.
- A Rails application set up in API mode (from previous guides).

#### **1. Understanding Error Messages**

When an error occurs in a Rails application, Rails provides detailed error messages and stack traces.

- **Stack Trace:** Shows the sequence of method calls leading up to the error.
- **Common Errors:** Syntax errors, missing routes, database issues, etc.

**Example Error Message:**

```
NameError (undefined local variable or method `name' for #<User:0x00007fae9c1b8c90>):
```

- **Interpreting:** The error is a `NameError` indicating that `name` is not defined for the `User` object.

#### **2. Using the Rails Logger**

**What is the Logger?**

- The Rails logger provides a way to output messages to the console or log files.
- Useful for tracking the flow of your application and values of variables.

**How to Use:**

- In your controller or model, you can add:

  ```ruby
  Rails.logger.debug "This is a debug message"
  ```

- **Log Levels:** `debug`, `info`, `warn`, `error`, `fatal`, `unknown`.

**Example:**

```ruby
def create
  user = User.new(user_params)
  Rails.logger.debug "User params: #{user_params.inspect}"
  if user.save
    render json: user, status: :created
  else
    render json: user.errors, status: :unprocessable_entity
  end
end
```

#### **3. Using `byebug`**

**What is `byebug`?**

- A debugging tool that allows you to pause code execution and inspect the state of your application at any point.

**Installing `byebug`:**

- Add `gem 'byebug'` to your `Gemfile` (usually included by default in development).

- Run:

  ```bash
  bundle install
  ```

**How to Use:**

- Insert `byebug` in your code where you want to start debugging.

**Example:**

```ruby
def show
  user = User.find(params[:id])
  byebug
  render json: user
end
```

**Using the `byebug` Console:**

- **Commands:**
  - `next` or `n`: Move to the next line.
  - `step` or `s`: Step into a method call.
  - `continue` or `c`: Resume program execution.
  - `list` or `l`: List the code around the current line.
  - `display variable_name`: Display the value of a variable.
  - `exit`: Exit the debugger.

**Example Session:**

```bash
(byebug) user
#<User id: 1, name: "Alice", email: "alice@example.com">
(byebug) user.email
"alice@example.com"
(byebug) user.name = "Bob"
"Bob"
(byebug) continue
```

#### **4. Debugging with Tests**

- Write tests to reproduce bugs.
- Use tests to verify that bugs are fixed.

#### **5. Common Debugging Tips**

- **Check the Rails Server Logs:** Run your server with `rails server` and watch the terminal for errors.
- **Inspect Parameters:** Ensure that the parameters being passed are as expected.
- **Database Queries:** Use `puts` or `Rails.logger.debug` to output the SQL queries being run.

---
