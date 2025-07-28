# GitHub Setup Guide

## Step 1: Sign Up for GitHub

1. Visit [GitHub](https://github.com/) and sign up for a free account.
2. After signing up, confirm your email address by following the instructions sent to your inbox.

---

## Step 2: Install Git

### Mac:
1. Open **Terminal** (you can find it by searching in Spotlight).
2. Type the following command to install Git via Homebrew:
   ```bash
   brew install git
   ```

### Windows (using WSL2):
1. First, set up WSL2 by following [these instructions](https://docs.microsoft.com/en-us/windows/wsl/install).
2. Open **Ubuntu** (inside WSL) and type:
   ```bash
   sudo apt update
   sudo apt install git
   ```

### Linux (Ubuntu/Debian):
1. Open **Terminal** and type:
   ```bash
   sudo apt update
   sudo apt install git
   ```

After installation, verify Git by typing:
```bash
git --version
```
You should see the version of Git installed.

---

## Step 3: Configure Git

Once Git is installed, you’ll need to configure it with your GitHub account information.

1. Open a terminal and run the following commands, replacing `Your Name` and `your-email@example.com` with your details:

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your-email@example.com"
   ```

2. Confirm your Git configuration:
   ```bash
   git config --global --list
   ```

---

## Step 4: Generate SSH Keys (for Secure GitHub Authentication)

### Mac and Linux (Ubuntu):
1. Open **Terminal** and type:
   ```bash
   ssh-keygen -t ed25519 -C "your-email@example.com"
   ```
   This command will generate a new SSH key.

2. When prompted to "Enter a file in which to save the key," press **Enter** to accept the default file location.

3. If you want, you can add a passphrase for an extra layer of security (you can skip by pressing **Enter**).

4. Add the SSH key to the SSH agent:
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

5. Copy the SSH key to your clipboard:
   ```bash
   pbcopy < ~/.ssh/id_ed25519.pub  # Mac
   cat ~/.ssh/id_ed25519.pub  # Linux (manually copy this output)
   ```

### Windows (using WSL2):
1. Inside **Ubuntu**, generate the SSH key:
   ```bash
   ssh-keygen -t ed25519 -C "your-email@example.com"
   ```

2. Follow the same process to save the key, adding a passphrase if desired.

3. Start the SSH agent:
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

4. Copy the SSH key to the clipboard:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   Then manually copy the output.

---

## Step 5: Add SSH Key to GitHub

1. Go to [GitHub SSH Settings](https://github.com/settings/keys).
2. Click **New SSH key**.
3. In the "Title" field, name your SSH key (e.g., "My Laptop SSH Key").
4. Paste your copied SSH key into the "Key" field.
5. Click **Add SSH key**.

To test if your SSH connection works, type in your terminal:
```bash
ssh -T git@github.com
```
If successful, you’ll get a message like:
> Hi username! You've successfully authenticated, but GitHub does not provide shell access.

---

## Step 6: Clone a GitHub Repository

Now that Git is configured and connected to GitHub, you can clone a repository.

1. Open a terminal and navigate to the directory where you want to clone the project.
   ```bash
   cd ~/Projects  # or wherever you store your projects
   ```

2. Clone a repository by running:
   ```bash
   git clone git@github.com:username/repository-name.git
   ```
   Replace `username` with the GitHub username and `repository-name` with the name of the repository you want to clone.

---

## Step 7: Creating a New Repository

1. Go to GitHub and click the **New repository** button on your dashboard.
2. Enter a repository name and optional description.
3. Choose whether the repository will be public or private.
4. You can add a README file, `.gitignore`, or license if you want.
5. Click **Create repository**.

---

## Step 8: Push Local Code to GitHub

### If you already have local files you'd like to push to GitHub:
1. Initialize a new Git repository:
   ```bash
   git init
   ```

2. Add your project files to the repository:
   ```bash
   git add .
   ```

3. Commit the changes:
   ```bash
   git commit -m "Initial commit"
   ```

4. Link your local repository to the GitHub repository (replace with your repository’s actual URL):
   ```bash
   git remote add origin git@github.com:username/repository-name.git
   ```

5. Push the changes to GitHub:
   ```bash
   git push -u origin main
   ```

---

## Step 9: Basic Git Commands

- **Check the status of your repository:**
  ```bash
  git status
  ```

- **View the commit history:**
  ```bash
  git log
  ```

- **Add new or changed files to the staging area:**
  ```bash
  git add <filename>  # or git add . to add everything
  ```

- **Commit your changes with a message:**
  ```bash
  git commit -m "Your commit message"
  ```

- **Push your code to GitHub:**
  ```bash
  git push
  ```

- **Pull the latest changes from the remote repository:**
  ```bash
  git pull
  ```

---

## Step 10: Branching (Optional)

- **Create a new branch:**
  ```bash
  git checkout -b feature-branch
  ```

- **Switch to an existing branch:**
  ```bash
  git checkout branch-name
  ```

- **Merge a branch into the main branch:**
  ```bash
  git checkout main
  git merge feature-branch
  ```

- **Delete a branch after merging:**
  ```bash
  git branch -d feature-branch
  ```

---
