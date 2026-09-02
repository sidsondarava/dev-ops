# Comprehensive Git Cheat Sheet 🚀

A structured, practical reference guide covering fundamental to advanced Git commands, real-world workflows, and best practices.

---

## 📑 Table of Contents
1. [Core Architecture & Concepts](#1-core-architecture--concepts)
2. [Configuration & Initial Setup](#2-configuration--initial-setup)
3. [Starting & Cloning Repositories](#3-starting--cloning-repositories)
4. [Basic Snapshotting & Staging](#4-basic-snapshotting--staging)
5. [Reviewing History & Changes](#5-reviewing-history--changes)
6. [Branching & Switching](#6-branching--switching)
7. [Merging & Conflict Resolution](#7-merging--conflict-resolution)
8. [Rebasing & Cherry-Picking](#8-rebasing--cherry-picking)
9. [Working with Remotes & Collaboration](#9-working-with-remotes--collaboration)
10. [Stashing (Temporary Workspace)](#10-stashing-temporary-workspace)
11. [Undoing Changes & History Reset](#11-undoing-changes--history-reset)
12. [Tagging & Releases](#12-tagging--releases)
13. [Ignoring Files (`.gitignore`)](#13-ignoring-files-gitignore)
14. [Pro Tips & Practical Aliases](#14-pro-tips--practical-aliases)

---

## 1. Core Architecture & Concepts

Git tracks files across four primary areas:

```text
Working Directory  ──(git add)──>  Staging Area (Index)  ──(git commit)──>  Local Repo (.git)  ──(git push)──>  Remote Repo
      │                                   │                                      │                                 │
      └─── Unstaged edits                 └─── Prepared for commit               └─── Committed history            └─── Shared (GitHub/GitLab)
```

- **Working Directory**: Your local workspace on the file system where you edit files.
- **Staging Area (Index)**: A staging zone holding snapshot preparations before saving.
- **Local Repository**: Committed milestones stored locally in `.git`.
- **Remote Repository**: The central server (GitHub, GitLab, Bitbucket) used for collaboration.

---

## 2. Configuration & Initial Setup

Set up your identity and preferences once per machine.

### Commands & Examples

```bash
# Set user name globally
git config --global user.name "Your Name"

# Set email address globally (use the one linked to your Git hosting platform)
git config --global user.email "your.email@example.com"

# Set default branch name for new repositories to 'main'
git config --global init.defaultBranch main

# Set your default text editor (e.g., nano, vim, code)
git config --global core.editor "code --wait"

# Enable colored command line output
git config --global color.ui auto

# List all configured settings
git config --list --show-origin
```

---

## 3. Starting & Cloning Repositories

### Commands & Examples

```bash
# Initialize a brand new local repository in the current folder
git init

# Initialize a new repository inside a specific directory
git init my-awesome-project

# Clone a remote repository via HTTPS
git clone https://github.com/user/repository.git

# Clone a remote repository via SSH into a custom folder name
git clone git@github.com:user/repository.git local-folder-name

# Shallow clone: download only the latest commit to save bandwidth/disk space
git clone --depth 1 https://github.com/user/repository.git
```

---

## 4. Basic Snapshotting & Staging

### Commands & Examples

```bash
# Check working directory and staging area status
git status

# Check status in short/compact format
git status -s

# Stage a specific file
git add main.cpp

# Stage multiple specific files
git add src/utils.cpp include/utils.h

# Stage all modified, created, and deleted files in the entire repo
git add .

# Stage changes interactively (review chunk-by-chunk / patch mode)
git add -p

# Commit staged changes with a descriptive message
git commit -m "feat: add user authentication endpoint"

# Stage all tracked modified files and commit in one step (skips untracked files)
git commit -am "fix: correct timeout calculation"

# Amend the most recent commit (add missed changes or fix commit message)
git commit --amend -m "feat: add user authentication endpoint with token validation"

# Remove a file from both working directory and Git tracking
git rm old_script.py

# Untrack a file from Git without deleting it from your local disk
git rm --cached config.local.json

# Rename or move a tracked file
git mv old_name.cpp new_name.cpp
```

---

## 5. Reviewing History & Changes

### Commands & Examples

```bash
# Show complete commit history
git log

# Compact one-line commit log
git log --oneline

# Visual graph of branching and merge history
git log --oneline --graph --all --decorate

# Show the last 5 commits with file change statistics
git log -n 5 --stat

# Show commits filtering by author or message search
git log --author="Alice" --grep="database"

# Inspect unstaged changes (Working Directory vs. Staging Area)
git diff

# Inspect staged changes waiting to be committed (Staging Area vs. Last Commit)
git diff --staged

# Compare changes between two branches
git diff main..feature-login

# Inspect metadata and diff of a specific commit
git show 4a2b1c3

# Show line-by-line author and revision information for a file
git blame src/engine.cpp
```

---

## 6. Branching & Switching

### Commands & Examples

```bash
# List all local branches (current branch highlighted with *)
git branch

# List all local and remote-tracking branches
git branch -a

# Create a new branch without switching to it
git branch feature/payment-gateway

# Switch to an existing branch (modern syntax)
git switch feature/payment-gateway

# Create and switch to a new branch in one command (modern syntax)
git switch -c feature/user-profile

# Classic syntax for creating and switching
git checkout -b bugfix/login-redirect

# Rename the current branch
git branch -m feature/new-name

# Safely delete a branch (only if fully merged)
git branch -d feature/payment-gateway

# Force delete an unmerged branch
git branch -D feature/payment-gateway
```

---

## 7. Merging & Conflict Resolution

### Commands & Examples

```bash
# 1. Switch to the receiving branch
git switch main

# 2. Merge feature branch into current branch
git merge feature/user-profile

# Merge without fast-forwarding (creates a dedicated merge commit)
git merge --no-ff feature/user-profile

# If conflicts occur:
# - Git pauses and marks conflict blocks:
#   <<<<<<< HEAD (current branch)
#   =======
#   >>>>>>> feature/user-profile (incoming branch)

# Inspect files with unresolved conflicts
git status

# After manually resolving conflicts, stage the resolved files:
git add resolved_file.cpp

# Complete the merge
git commit -m "merge: resolve conflicts and merge feature/user-profile"

# Abort an ongoing merge and return to the state before merging began
git merge --abort
```

---

## 8. Rebasing & Cherry-Picking

Rebasing rewrites commit history to maintain a linear, clean project timeline.

### Commands & Examples

```bash
# Rebase current branch onto latest main
git switch feature/search
git rebase main

# Interactive rebase: squashing, editing, or reordering the last 3 commits
git rebase -i HEAD~3
# In the editor:
# pick  -> keep commit as-is
# reword -> edit commit message
# squash -> combine commit into previous commit
# drop   -> remove commit entirely

# If conflicts occur during rebase:
# 1. Fix conflicts in files
git add <resolved-file>
git rebase --continue

# Abort the rebase process and return to the original state
git rebase --abort

# Cherry-pick a specific commit from another branch into current branch
git cherry-pick 7f3a9bc
```

> ⚠️ **Rule of Thumb:** Never rebase commits that have already been pushed to a shared public branch.

---

## 9. Working with Remotes & Collaboration

### Commands & Examples

```bash
# List configured remote repositories with fetch/push URLs
git remote -v

# Add a new remote connection
git remote add origin https://github.com/user/project.git

# Rename a remote
git remote rename origin upstream

# Remove a remote
git remote remove upstream

# Fetch updates from remote without merging into local branches
git fetch origin

# Fetch and prune deleted remote branches locally
git fetch -p origin

# Pull remote changes and merge them into the current local branch
git pull origin main

# Pull using rebase instead of merge (avoids unnecessary merge commits)
git pull --rebase origin main

# Push current branch and set default upstream tracking
git push -u origin feature/auth

# Subsequent pushes on a tracked branch
git push

# Push all local tags to remote
git push origin --tags

# Delete a branch on the remote server
git push origin --delete feature/old-feature
```

---

## 10. Stashing (Temporary Workspace)

Safely shelf uncommitted edits without committing so you can switch branches cleanly.

### Commands & Examples

```bash
# Save uncommitted changes (tracked files) to stash with a message
git stash save "WIP: refactoring calculation loop"

# Include untracked files in the stash
git stash -u

# List all stashed changes
git stash list

# Apply the latest stash and keep it in the stash list
git stash apply

# Apply a specific stash from the list
git stash apply stash@{2}

# Apply the latest stash and immediately drop it from stash list
git stash pop

# Discard the latest stash
git stash drop

# Discard a specific stash
git stash drop stash@{1}

# Clear all stashed changes completely
git stash clear
```

---

## 11. Undoing Changes & History Reset

### Commands & Examples

```bash
# Discard uncommitted changes in a specific file (restore to HEAD state)
git restore file.cpp

# Unstage a file without losing local modifications
git restore --staged file.cpp

# Revert a published commit by creating a new inverse commit (safe for shared branches)
git revert 3d4e5f6

# Soft reset: undo the last commit, keep changes staged in index
git reset --soft HEAD~1

# Mixed reset (default): undo the last commit, keep changes in working directory (unstaged)
git reset --mixed HEAD~1

# Hard reset: completely discard last commit and all local changes (destructive!)
git reset --hard HEAD~1

# Clean untracked files and directories:
# Preview files to be deleted:
git clean -nd
# Force remove untracked files and folders:
git clean -fd

# Emergency recovery: inspect reference log of every HEAD change
git reflog
# Restore lost commit discovered via reflog:
git reset --hard HEAD@{4}
```

---

## 12. Tagging & Releases

Tags are permanent pointers marking specific release milestones (e.g., semantic versions).

### Commands & Examples

```bash
# List all existing tags
git tag

# List tags matching a pattern
git tag -l "v1.*"

# Create a lightweight tag on the current commit
git tag v1.0.0-light

# Create an annotated tag with author, date, and message (recommended for releases)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag a past commit using its SHA hash
git tag -a v0.9.0 9a1b2c3 -m "Beta release"

# View details and commit linked to a tag
git show v1.0.0

# Push a specific tag to remote
git push origin v1.0.0

# Push all local tags to remote
git push origin --tags

# Delete a tag locally
git tag -d v1.0.0

# Delete a tag on remote
git push origin --delete v1.0.0
```

---

## 13. Ignoring Files (`.gitignore`)

Create a `.gitignore` file in your repository root to prevent unwanted files from being tracked.

### Syntax & Example Patterns

```gitignore
# Comments start with '#'

# Ignore compiled binaries and objects
*.o
*.obj
*.exe
*.so

# Ignore OS system files
.DS_Store
Thumbs.db

# Ignore build output directories
build/
bin/
dist/

# Ignore dependency directories
node_modules/
vendor/

# Ignore environment files containing secrets
.env
.env.local

# Negation: ignore all .log files except important.log
*.log
!important.log
```

---

## 14. Pro Tips & Practical Aliases

Save time by defining shortcuts in your global `.gitconfig`.

```bash
# Set up handy aliases
git config --global alias.st "status -s"
git config --global alias.co "checkout"
git config --global alias.sw "switch"
git config --global alias.br "branch"
git config --global alias.ci "commit"
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Now you can use:
git st
git lg
git sw -c feature/quick-fix
```

### Common Troubleshooting Scenarios

| Problem | Cause | Quick Fix |
| :--- | :--- | :--- |
| **"Detached HEAD" state** | Checked out a commit directly instead of a branch | Run `git switch main` or create a branch from here: `git switch -c new-branch-name` |
| **Forgot to add a file in the last commit** | File left unstaged | Run `git add <file>` then `git commit --amend --no-edit` |
| **Pushed wrong commit / message to remote** | Early typo or error | Fix locally, then push with lease: `git push --force-with-lease` *(only on private feature branches)* |
| **Accidentally deleted or reset a branch** | Hard reset or deleted branch | Find the dangling commit SHA using `git reflog`, then checkout or branch from it: `git switch -c restored-branch <SHA>` |

---

*Keep this reference handy whenever you are coding, branching, or collaborating!*