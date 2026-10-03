# Essential Git Guide & Reference Manual

A comprehensive, practical guide to essential Git commands, their most useful flags, and real-world usage examples.

---

## 📌 Quick Reference Cheatsheet

| Command | Purpose | Primary Flags |
| :--- | :--- | :--- |
| [`git add`](#1-git-add) | Stage changes for the next commit | `-A`, `-p`, `-u` |
| [`git push`](#2-git-push) | Upload local commits to a remote repository | `-u`, `--force-with-lease`, `-d` |
| [`git pull`](#3-git-pull) | Fetch and integrate changes from a remote branch | `--rebase`, `--ff-only`, `--autostash` |
| [`Changing Origin URL`](#4-changing-the-origin-url) | Point the local repository to a new remote URL | `set-url`, `-v` |
| [`git stash`](#5-git-stash) | Temporarily shelve changes without committing | `push -m`, `pop`, `-u`, `list` |
| [`git revert`](#6-git-revert) | Create a new commit that inverts a previous commit | `--no-commit`, `-m` |
| [`git reset`](#7-git-reset) | Move branch HEAD and optionally alter staging / working tree | `--soft`, `--mixed`, `--hard` |
| [`git log`](#8-git-log) | Inspect the repository commit history | `--oneline`, `--graph`, `--stat`, `-n` |
| [`git diff`](#9-git-diff) | Compare changes across working tree, index, or branches | `--staged`, `--stat`, `branch1..branch2`, `-w` |
| [`git show`](#10-git-show) | Display metadata and content changes of a commit or object | `--stat`, `--name-only`, `<commit>:<path>` |

---

## 🛠️ Command Reference & Examples

### 1. `git add`
Stages modifications from the working directory into the index (staging area) preparing them for the next commit.

#### Most Useful Flags
- `-A`, `--all` : Stages all changes across the entire repository (new files, modifications, and deletions).
- `-p`, `--patch` : Interactively reviews chunks (hunks) of changes, allowing you to selectively stage parts of a modified file.
- `-u`, `--update` : Stages modified and deleted tracked files, completely ignoring untracked new files.

#### Practical Examples
```bash
# 1. Stage a specific file
git add bulletin-board-app/server.js

# 2. Review and stage hunks interactively (ideal for splitting unrelated edits)
git add -p

# 3. Stage all modified, created, and deleted files throughout the workspace
git add -A
```
> **Pro Tip**: Use `git add -p` before committing to avoid accidentally including debugging code or temporary console logs.

---

### 2. `git push`
Transfers committed changes from your local repository branch to the corresponding upstream branch on GitHub.

#### Most Useful Flags
- `-u`, `--set-upstream` : Links your local branch with the remote tracking branch, allowing shorthand `git push` and `git pull` in subsequent calls.
- `--force-with-lease` : A safer alternative to `--force`. Overwrites remote history only if no one else has pushed commits to the remote branch in the meantime.
- `-d`, `--delete` : Removes a remote branch from the repository.

#### Practical Examples
```bash
# 1. Push a newly created branch and establish tracking with origin
git push -u origin init

# 2. Push safely after an interactive rebase or squashing commits locally
git push --force-with-lease origin feature-login

# 3. Delete an obsolete remote branch after merging a PR
git push origin --delete old-feature-branch
```

---

### 3. `git pull`
Fetches changes from the remote repository and immediately integrates them into the current active branch.

#### Most Useful Flags
- `--rebase` : Re-applies your local commits on top of incoming remote commits, creating a clean, linear commit history without extraneous merge commits.
- `--ff-only` : Refuses to merge unless the incoming changes can be fast-forwarded, protecting your local history from accidental divergence.
- `--autostash` : Automatically saves uncommitted local modifications to the stash before pulling, then pops the stash after rebase finishes.

#### Practical Examples
```bash
# 1. Pull remote updates and maintain a clean linear commit graph
git pull --rebase origin main

# 2. Pull incoming updates safely when you have uncommitted changes in progress
git pull --rebase --autostash origin main

# 3. Ensure your local branch is strictly behind upstream before proceeding
git pull --ff-only origin main
```

---

### 4. Changing the Origin URL
Updates the network address (HTTPS or SSH) associated with the remote repository alias named `origin`.

#### Key Commands & Flags
- `git remote set-url origin <new-url>` : Updates the remote URL endpoint for `origin`.
- `-v`, `--verbose` : Lists current remote names alongside their fetch and push target URLs.

#### Practical Examples
```bash
# 1. Inspect current remote URLs
git remote -v
# Output:
# origin  https://github.com/riku05190/riku-nakamura-personal.git (fetch)
# origin  https://github.com/riku05190/riku-nakamura-personal.git (push)

# 2. Switch remote repository from HTTPS to SSH format
git remote set-url origin git@github.com:riku05190/riku-nakamura-personal.git

# 3. Point to BYU-Idaho ITM 350 organization repository
git remote set-url origin git@github.com:byui-itm350-w25/riku-nakamura-personal.git

# 4. Verify that the new URL was registered successfully
git remote -v
```

---

### 5. `git stash`
Temporarily shelves uncommitted changes (both staged and unstaged) in a local storage stack so you can work on something else with a clean working tree.

#### Most Useful Flags & Subcommands
- `push -m "<message>"` : Saves local modifications with an informative description for later retrieval.
- `-u`, `--include-untracked` : Stashes newly created (untracked) files in addition to tracked file edits.
- `pop` : Applies the latest shelved changes back to the working tree and removes them from the stash stack.
- `list` : Displays all stashed snapshots along with their index identifier (`stash@{n}`).

#### Practical Examples
```bash
# 1. Shelve uncommitted work including untracked files with a clear note
git stash push -u -m "WIP: container configuration adjustments"

# 2. View all saved stash snapshots
git stash list
# Output: stash@{0}: On init: WIP: container configuration adjustments

# 3. Restore your stashed edits and remove the snapshot from storage
git stash pop
```

---

### 6. `git revert`
Records a new commit that applies the exact inverse diff of an existing commit. This is the safest way to undo changes on public shared branches.

#### Most Useful Flags
- `-n`, `--no-commit` : Applies the inverted changes directly to your working tree and staging area without immediately creating a commit, allowing multi-commit rollbacks into a single revision.
- `-m <parent-number>` : Specifies the parent commit number when reverting a merge commit (typically `-m 1` to keep mainline changes).

#### Practical Examples
```bash
# 1. Invert and undo a specific faulty commit while preserving project history
git revert a1b2c3d

# 2. Stage the inverse of the last 2 commits without automatically committing
git revert -n HEAD~1
git revert -n HEAD
git commit -m "Rollback unstable API features"
```

---

### 7. `git reset`
Moves the current branch pointer (HEAD) backward to a specified target commit. Changes how staging and the working tree are affected based on the mode flag.

#### Most Useful Flags
- `--soft` : Moves HEAD back to the target commit while leaving all files in your staging area untouched. Ideal for re-grouping recent commits.
- `--mixed` *(Default)* : Moves HEAD and resets the staging area, but preserves all your modified files in the working directory.
- `--hard` : Moves HEAD and resets both the staging area and working directory, completely discarding all uncommitted changes. **Use with caution!**

#### Practical Examples
```bash
# 1. Undo the last commit, keeping all changes staged ready for a rewrite
git reset --soft HEAD~1

# 2. Unstage a file you added accidentally without discarding any modifications
git reset HEAD bulletin-board-app/test.log

# 3. Discard local modifications completely and align strictly with remote
git reset --hard origin/main
```

---

### 8. `git log`
Navigates and inspects the chronological commit history of the repository.

#### Most Useful Flags
- `--oneline` : Formats each commit entry as a single concise line (condensed SHA-1 hash and commit subject).
- `--graph` : Renders an ASCII text-based graphical representation of branch divergence and merge topology.
- `--stat` : Lists files modified, lines inserted, and lines deleted in each commit.
- `-n <number>` : Restricts the output to a specified count of recent commits.

#### Practical Examples
```bash
# 1. View a compact, visualized branch graph of the last 5 commits
git log --graph --oneline --decorate -n 5

# 2. View detailed line statistics of recent project commits
git log --stat -n 3

# 3. Filter commits by author and search keyword in commit messages
git log --author="Riku" --grep="docker" --oneline
```

---

### 9. `git diff`
Compares differences between file revisions, the staging area, working tree, and across separate branches.

#### Most Useful Flags
- `--staged` / `--cached` : Shows modifications that have been added to the staging area against the latest commit.
- `--stat` : Produces a high-level summary of changed file paths and inserted/deleted line counts instead of raw diff hunks.
- `branch1..branch2` : Shows all differences introduced between two branch tips.
- `-w`, `--ignore-all-space` : Suppresses whitespace differences (indentation/tab changes) to isolate functional code modifications.

#### Practical Examples
```bash
# 1. Review what is currently staged before running git commit
git diff --staged

# 2. Compare functional changes between your current branch and main
git diff main..init --stat

# 3. Check modifications in the working tree ignoring formatting whitespace
git diff -w bulletin-board-app/Dockerfile
```

---

### 10. `git show`
Displays comprehensive metadata and the full code diff for a specific Git object (such as a commit, tag, or blob).

#### Most Useful Flags
- `--stat` : Shows the commit log summary and modified file statistics without printing full diff lines.
- `--name-only` : Lists only the names of files touched in the given commit.
- `<commit>:<filepath>` : Outputs the exact contents of a file as it existed at that specific commit without switching branches.

#### Practical Examples
```bash
# 1. Inspect the full details and diff of the most recent commit
git show HEAD

# 2. View file change statistics for an earlier commit
git show --stat a1b2c3d

# 3. Inspect a file from an older commit directly in the terminal
git show HEAD~2:bulletin-board-app/package.json
```

---

## 🏆 Summary: Essential Git Best Practices

1. **Commit Early and Often**: Write clear, descriptive commit messages outlining *why* a change was made.
2. **Review Before Committing**: Run `git diff --staged` and `git status` before executing `git commit`.
3. **Protect Main Branch**: Always develop features in dedicated branches (e.g., `init` or `feature/*`) and integrate via Pull Requests.
4. **Prefer Rebase for Clean History**: Use `git pull --rebase` to avoid cluttering branch history with unnecessary merge commits.
