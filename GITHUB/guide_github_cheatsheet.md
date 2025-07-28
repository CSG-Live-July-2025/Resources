# Git & GitHub Cheat Sheet

## 1. Setting Up

1. **Configure Your User**
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```
   *Sets the name and email used for all Git commits.*

2. **Initialize a Repository**
   ```bash
   git init
   ```
   *Creates a new Git repository in your current folder.*

3. **Clone an Existing Repository**
   ```bash
   git clone <repository-url>
   ```
   *Makes a local copy of a remote repository.*

---

## 2. Basic Workflow

1. **Check Status**
   ```bash
   git status
   ```
   *Shows changed files, whether they’re staged, and other helpful info.*

2. **Stage Files**
   ```bash
   git add <file>       # Stage a single file
   git add .            # Stage all changed files in the current directory
   ```
   *Adds changes to the “staging area” so they can be included in the next commit.*

3. **Commit Changes**
   ```bash
   git commit -m "Commit message here"
   ```
   *Commits the staged changes to the repository’s history.*

4. **Push Changes (to remote)**
   ```bash
   git push origin <branch-name>
   ```
   *Uploads your local branch commits to the remote repository.*

---

## 3. Branching & Merging

1. **Create a New Branch**
   ```bash
   git checkout -b <new-branch-name>
   ```
   *Creates and switches you to a new branch.*

2. **Switch Branches**
   ```bash
   git checkout <branch-name>
   ```
   *Switches to an existing branch.*

3. **Merge Branches**
   ```bash
   # First, ensure you are on the branch you want to merge into (e.g., main)
   git checkout main
   git merge <branch-name>
   ```
   *Merges `<branch-name>` into your current branch.*  
   *If there are no conflicts, your merge is complete.*

4. **Delete a Branch** (after merging)
   ```bash
   git branch -d <branch-name>
   ```
   *Deletes a local branch you no longer need.*

---

## 4. Dealing with Merge Conflicts

### 4.1 Automatic Conflict Detection
When you run a merge or pull that Git can’t automatically resolve, it will pause and report a conflict. For example:
```bash
git merge <branch-name>
# or
git pull origin <branch-name>
```
You may see a message like:
```
CONFLICT (content): Merge conflict in app.js
Automatic merge failed; fix conflicts and then commit the result.
```

### 4.2 Inspecting Conflicts
- **Check Status**:
  ```bash
  git status
  ```
  *Git lists files under “Unmerged paths.” These files need manual resolution.*

### 4.3 Resolving the Conflict
1. **Open the Conflicting File(s) in an Editor**  
   Look for lines marked by:
   ```
   <<<<<<< HEAD
   (Your local changes)
   =======
   (Incoming changes from the other branch)
   >>>>>>> branch-name
   ```
2. **Decide What to Keep**  
   - Remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
   - Keep or combine the code from both sides as needed.
   - Save the file.

### 4.4 Marking Conflicts as Resolved
1. **Stage the Resolved File(s)**:
   ```bash
   git add <file>
   ```
   or:
   ```bash
   git add .
   ```
   *if you’ve resolved multiple files.*

2. **Commit the Resolution**:
   ```bash
   git commit -m "Resolve merge conflicts"
   ```
   *Creates a new commit with your resolution.*

### 4.5 Done!
- **Verify**:
  ```bash
  git status
  ```
  *Should show “nothing to commit” and confirm a clean merge.*

---

## 5. Updating / Reverting Changes

1. **Pull Latest Changes**
   ```bash
   git pull origin <branch-name>
   ```
   *Fetches and merges changes from the remote branch into your local branch.*

2. **Discard All Local Changes in a File**
   ```bash
   git checkout -- <file>
   ```
   *Reverts `<file>` to the last committed state.*

   Or if you want to discard **all** uncommitted changes in your working directory:
   ```bash
   git checkout .
   ```
   *Use with caution! Any uncommitted changes will be lost.*

3. **View History**
   ```bash
   git log
   ```
   *Shows the commit history for the current branch.*

4. **Compare Changes**
   ```bash
   git diff
   git diff <branch1> <branch2>
   ```
   *Shows line-by-line changes in your working directory or between branches.*

---

## 6. Basic Workflow Example

1. **Clone** a repo:
   ```bash
   git clone https://github.com/codeschoolofguam/project.git
   cd project
   ```

2. **Create a Branch** for a new feature:
   ```bash
   git checkout -b new-feature
   ```

3. **Work on your code**, then **stage & commit** changes:
   ```bash
   git add .
   git commit -m "Add initial new feature"
   ```

4. **Pull** latest changes from `main` to stay up-to-date and reduce conflicts:
   ```bash
   git checkout main
   git pull origin main
   git checkout new-feature
   git merge main
   # Resolve conflicts if any, then commit
   ```

5. **Push** the new feature branch and **create a Pull Request**:
   ```bash
   git push origin new-feature
   # Then open GitHub and create a Pull Request
   ```

6. **Merge** the Pull Request into `main` after code review.
   - Alternatively, from the command line, switch to `main` and run:
     ```bash
     git merge new-feature
     ```

---

## 7. Additional Git Commands

### 7.1 Stashing Changes

- **git stash**  
  Temporarily saves (or “stashes”) changes that are not ready to be committed, allowing you to switch branches or pull updates without committing half-done work.
  ```bash
  git stash
  ```
- **git stash pop**  
  Restores the stashed changes back into your working directory (and removes them from the stash).
  ```bash
  git stash pop
  ```

### 7.2 Resetting Changes

- **git reset HEAD <file>**  
  Unstages a file you previously staged, but leaves working directory changes in place.
  ```bash
  git reset HEAD index.html
  ```
- **git reset --hard <commit>**  
  Moves your current branch pointer to the specified `<commit>` and discards all changes in the staging area and working directory.  
  **Use with caution** because it can permanently remove commits if you’re rewriting history.
  ```bash
  git reset --hard HEAD~1
  # HEAD~1 is “one commit before HEAD”
  ```

### 7.3 Reverting Commits

- **git revert <commit-hash>**  
  Creates a new commit that undoes the changes introduced by `<commit-hash>` without rewriting project history.
  ```bash
  git revert 123abc
  ```

### 7.4 Working with Remote Branches

- **git fetch**  
  Retrieves changes and updates references from a remote repository, but does not merge them automatically.
  ```bash
  git fetch origin
  ```
- **git push -u origin <branch-name>**  
  Sets the default remote branch for subsequent pushes/pulls, so you don’t have to specify it every time.
  ```bash
  git push -u origin feature/new-ui
  ```

### 7.5 Renaming or Removing Files

- **git mv <old-path> <new-path>**  
  Safely rename a file or folder so Git tracks it as a rename instead of a delete + new file.
  ```bash
  git mv old_name.txt new_name.txt
  ```
- **git rm <file>**  
  Removes a file from your working directory and stages the deletion for commit.
  ```bash
  git rm old_file.txt
  ```

### 7.6 Tagging

- **git tag <tag-name>**  
  Mark specific points in history (often used for releases like v1.0, v2.0, etc.).
  ```bash
  git tag v1.0
  ```
- **git push origin <tag-name>**  
  Push a specific tag to the remote repository.
  ```bash
  git push origin v1.0
  ```

### 7.7 Cherry-Picking

- **git cherry-pick <commit-hash>**  
  Copies a commit from one branch to another. Useful if a single commit contains a bug fix needed in multiple branches.
  ```bash
  git cherry-pick 123abc
  ```

---

## 8. Additional Tips & Best Practices

1. **Use Branches for Everything**  
   Create a separate branch for each feature, fix, or experiment. This keeps `main` clean.

2. **`git pull --rebase` vs. `git pull`**  
   - `git pull` does a default merge, potentially creating extra merge commits.  
   - `git pull --rebase` “replays” your local commits on top of the remote branch, keeping a cleaner, more linear history. (Check your team’s policy.)

3. **Avoid Force Push in Shared Repos**  
   - `git push --force` rewrites history on the remote branch. Use extreme caution or avoid it unless you know exactly what you’re doing.

4. **Branch Naming Conventions**  
   - Keep it short, descriptive, and consistent. Examples:  
     - `feat/login-page`  
     - `bugfix/dashboard-404`  
     - `docs/readme-updates`

5. **Commit Message Best Practices**  
   - **Title line** (50 characters max): Summary of the changes.  
   - **Optional body** (72 characters max/line): The “why” behind the changes or additional context.

6. **Leverage `.gitignore`**  
   - Keep large binary files, build artifacts, secrets, or OS-specific files out of source control.

7. **Short Status & Concise Logs**  
   - `git status -s` for a short status.  
   - `git log --oneline` for a concise, one-line history view.

8. **View All Branches**  
   - Local branches: `git branch`  
   - Remote branches: `git branch -r`  
   - Both: `git branch -a`

9. **Commit Often & Meaningfully**  
   - Frequent small commits are easier to review and revert if something goes wrong.

10. **Resolve Merge Conflicts ASAP**  
    - The longer you wait, the more complicated they can become.

---

## 9. Example Advanced Workflow

1. **Stash Changes** if you need to switch context quickly:
   ```bash
   git stash
   ```
2. **Check out** a different branch to fix a quick bug:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b bugfix/urgent-typo
   ```
3. **Fix & commit**:
   ```bash
   git add .
   git commit -m "Fix urgent typo on homepage"
   git push origin bugfix/urgent-typo
   ```
4. **Open a PR**, merge it into `main` when ready.
5. **Pop stashed changes** back on your original branch:
   ```bash
   git checkout your-original-branch
   git stash pop
   ```

---

## 10. Additional Resources

- **[Official Git Documentation](https://git-scm.com/doc)**
- **[GitHub Guides](https://guides.github.com/)**
- **[Pro Git eBook (Free)](https://git-scm.com/book/en/v2)**
