# Git Command Guide

A concise guide to the essential Git commands, their most useful flags, and practical usage examples.

---

### 1. `git add`
Stages modifications from the working directory for the next commit.
- **Useful Flags**:
  - `-A` (`--all`): Stages all changes across the repository (new, modified, and deleted files).
  - `-p` (`--patch`): Interactively reviews and stages specific parts (hunks) of modified files.
- **Example**:
  ```bash
  git add index.js
  git add -A
  ```

---

### 2. `git push`
Uploads local commits to a remote repository branch.
- **Useful Flags**:
  - `-u` (`--set-upstream`): Links the local branch to the remote branch for future shorthand pushes.
  - `-d` (`--delete`): Deletes a specified remote branch.
- **Example**:
  ```bash
  git push -u origin init
  git push origin -d feature-branch
  ```

---

### 3. `git pull`
Fetches changes from a remote branch and integrates them into the current branch.
- **Useful Flags**:
  - `--rebase`: Applies local commits on top of incoming commits to maintain a clean linear history.
  - `--autostash`: Automatically stashes uncommitted local changes before pulling and reapplies them after.
- **Example**:
  ```bash
  git pull --rebase origin main
  ```

---

### 4. Changing Origin URL (`git remote set-url`)
Updates the remote repository URL (e.g., switching between HTTPS and SSH).
- **Useful Flags / Usage**:
  - `origin <url>`: Sets the new target address for the `origin` remote.
  - `-v`: Displays all configured remote names and their URLs.
- **Example**:
  ```bash
  git remote -v
  git remote set-url origin git@github.com:riku05190/riku-nakamura-personal.git
  ```

---

### 5. `git stash`
Temporarily shelves uncommitted changes so you can work on a clean working tree.
- **Useful Flags / Commands**:
  - `push -m "<msg>"`: Saves changes with an optional descriptive message.
  - `-u` (`--include-untracked`): Includes newly created (untracked) files in the stash.
  - `pop`: Re-applies the most recent stash and removes it from the list.
- **Example**:
  ```bash
  git stash push -u -m "work in progress"
  git stash pop
  ```

---

### 6. `git revert`
Safely undoes changes from an earlier commit by creating a new inverse commit.
- **Useful Flags**:
  - `-n` (`--no-commit`): Applies the inverted diff to the working directory without auto-committing.
- **Example**:
  ```bash
  git revert <commit-hash>
  ```

---

### 7. `git reset`
Moves the current branch pointer (HEAD) backward to an earlier commit.
- **Useful Flags**:
  - `--soft`: Moves HEAD back while keeping changes staged.
  - `--mixed` *(Default)*: Moves HEAD and unstages changes, keeping files in the working directory.
  - `--hard`: Completely discards all uncommitted modifications in both staging and working directory.
- **Example**:
  ```bash
  git reset --soft HEAD~1
  git reset --hard origin/main
  ```

---

### 8. `git log`
Displays the chronological commit history of the repository.
- **Useful Flags**:
  - `--oneline`: Condenses each commit entry into a single line (short SHA + message).
  - `--graph`: Draws an ASCII graph showing branch divergence and merges.
  - `-n <number>`: Restricts output to the specified number of recent commits.
- **Example**:
  ```bash
  git log --oneline --graph -n 5
  ```

---

### 9. `git diff`
Compares changes between the working directory, staging area, or branches.
- **Useful Flags**:
  - `--staged`: Shows differences between staged changes and the last commit.
  - `--stat`: Displays a concise summary of modified files and line counts.
- **Example**:
  ```bash
  git diff --staged
  git diff main..init --stat
  ```

---

### 10. `git show`
Shows detailed metadata and code diffs for a specific commit or object.
- **Useful Flags**:
  - `--stat`: Shows a summary of file changes without full diff text.
  - `--name-only`: Lists only the names of modified files.
- **Example**:
  ```bash
  git show HEAD
  git show --name-only <commit-hash>
  ```
