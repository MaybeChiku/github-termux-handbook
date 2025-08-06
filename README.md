# GitHub Guide for Termux

> Complete guide for Git and GitHub operations on Android using Termux

## 📱 Required Apps
- **[Termux](https://f-droid.org/en/packages/com.termux/)** - Terminal emulator for Android
- **[Material Files](https://play.google.com/store/apps/details?id=me.zhanghai.android.files)** - File manager with path copying


## ⚙️ Initial Setup

### 1. System Preparation
```bash
# Update system packages
apt update && apt upgrade -y

# Enable storage access (IMPORTANT!)
termux-setup-storage

# Install essential tools
pkg install git gh nano curl wget -y
```

### 2. GitHub Authentication
```bash
# Login to GitHub
gh auth login

# Follow the prompts:
# - Select GitHub.com
# - Choose HTTPS or SSH
# - Authenticate via web browser or token
```

### 3. Configure Git Identity
```bash
# Set your email and username
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor nano
```


## 📤 Upload Files to GitHub

### Method 1: Upload New Project
```bash
# Navigate to your project folder
cd /storage/emulated/0/Download/your-project
# or use: cd ~/storage/downloads/your-project

# Initialize Git repository
git init

# Add all files
git add .

# Create first commit
git commit -m "🎉 Initial commit"

# Create GitHub repository
gh repo create your-project-name --public

# Connect and push to GitHub
git remote add origin https://github.com/yourusername/your-project-name.git
git push -u origin main
```

### Method 2: Upload to Existing Repository
```bash
# Clone existing repository
git clone https://github.com/username/repository-name.git
cd repository-name

# Copy your files here
cp -r ~/storage/downloads/your-files/* .

# Stage and commit
git add .
git commit -m "📁 Add new files"

# Push to GitHub
git push origin main
```

### Method 3: Quick File Upload
```bash
# Add specific files
git add filename.txt folder/

# Commit with message
git commit -m "✨ Add new feature"

# Push changes
git push
```


## ↩️ Reverse & Undo Changes

### Undo Last Commit (Keep Files)
```bash
git reset --soft HEAD~1
```

### Undo Last Commit (Delete Changes)
```bash
git reset --hard HEAD~1
```

### Undo Multiple Commits
```bash
git reset --hard HEAD~3  # Undo last 3 commits
```

### Revert Specific Commit
```bash
# Find commit hash
git log --oneline

# Revert commit (creates new commit)
git revert abc1234
```

### Reset to Specific Commit
```bash
git reset --hard abc1234
git push -f origin main  # Force push if needed
```

### Undo File Changes
```bash
# Undo changes to specific file
git checkout -- filename.txt

# Undo all file changes
git checkout -- .
```


## 🔄 Push, Pull & Sync

### Basic Operations
```bash
# Push changes
git push

# Pull latest changes
git pull

# Force push (use carefully!)
git push -f origin main

# Fetch without merging
git fetch origin
```

### Sync with Remote
```bash
# Check for changes
git status

# Pull and push in sequence
git pull origin main
git push origin main

# Pull with rebase (cleaner history)
git pull --rebase origin main
```

### Handle Merge Conflicts
```bash
# When conflicts occur during pull
git status  # Shows conflicted files

# Edit files to resolve conflicts
nano conflicted-file.txt

# After resolving conflicts
git add .
git commit -m "🔀 Resolve merge conflicts"
git push
```


## 🌿 Branch Management

### Create & Switch Branches
```bash
# Create new branch
git checkout -b feature-branch

# Switch to existing branch
git checkout main

# Create and switch in one command
git switch -c new-feature
```

### Branch Operations
```bash
# List all branches
git branch -a

# Rename current branch
git branch -m new-name

# Delete branch locally
git branch -d branch-name

# Delete branch on GitHub
git push origin --delete branch-name

# Push new branch to GitHub
git push -u origin feature-branch
```

### Merge Branches
```bash
# Switch to main branch
git checkout main

# Merge feature branch
git merge feature-branch

# Push merged changes
git push origin main
```


## 🔍 Recovery & History

### View History
```bash
# View commit history
git log --oneline

# View detailed history
git log --graph --pretty=format:'%h %s (%an)'

# View file history
git log --follow filename.txt
```

### Recover Lost Commits
```bash
# View all actions (including deleted commits)
git reflog

# Recover deleted commit
git checkout abc1234
git checkout -b recovery-branch

# Cherry-pick specific commit
git cherry-pick abc1234
```

### Find Changes
```bash
# Search in files
git grep "search term"

# Search in commit messages
git log --grep="bug fix"

# Search by author
git log --author="username"
```


## 💾 Stash & Temporary Save

### Save Work Temporarily
```bash
# Save current changes
git stash

# Save with custom message
git stash save "work in progress"

# List all stashes
git stash list
```

### Restore Stashed Work
```bash
# Apply latest stash
git stash pop

# Apply specific stash
git stash apply stash@{1}

# Delete stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```


## 🏷️ Tags & Releases

### Create Tags
```bash
# Simple tag
git tag v1.0.0

# Annotated tag with message
git tag -a v1.0.0 -m "Version 1.0.0 release"

# Push tags to GitHub
git push origin --tags
```

### Manage Tags
```bash
# List tags
git tag

# Delete tag locally
git tag -d v1.0.0

# Delete tag on GitHub
git push origin --delete v1.0.0

# Checkout specific tag
git checkout v1.0.0
```


## 🔧 Repository Management

### Repository Information
```bash
# Check repository status
git status

# View remote URLs
git remote -v

# Change remote URL
git remote set-url origin https://github.com/user/new-repo.git

# View configuration
git config --list
```

### Clean Repository
```bash
# Remove untracked files (preview)
git clean -n

# Remove untracked files
git clean -f

# Remove untracked directories
git clean -fd

# Optimize repository
git gc --aggressive
```


## ⚠️ Emergency & Troubleshooting

### Common Errors & Solutions

#### Error: "src refspec main does not match any"
```bash
git checkout -b main
git push -u origin main
```

#### Error: Large file upload
```bash
git config http.postBuffer 524288000
git push
```

#### Authentication Problems
```bash
gh auth logout
gh auth login
```

#### Repository Corruption
```bash
git fsck --full
git gc --aggressive
```

### Reset Everything
```bash
# Reset to remote state
git fetch origin
git reset --hard origin/main

# Nuclear option (lose all local changes)
git reset --hard HEAD
git clean -fd
```


## 🤝 Collaboration

### Fork & Pull Requests
```bash
# Fork repository
gh repo fork owner/repository

# Create pull request
gh pr create --title "Feature name" --body "Description"

# List pull requests
gh pr list

# Checkout pull request locally
gh pr checkout 123
```

### Work with Forks
```bash
# Add upstream remote
git remote add upstream https://github.com/original/repo.git

# Sync with upstream
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```


## 📂 File Operations

### File Management
```bash
# Stage specific files
git add file1.txt file2.txt

# Stage by pattern
git add *.py

# Unstage files
git reset filename.txt

# Remove file from git
git rm filename.txt

# Rename/move file
git mv oldname.txt newname.txt
```

### View Changes
```bash
# View unstaged changes
git diff

# View staged changes
git diff --cached

# Compare with previous commit
git diff HEAD~1

# View changes in specific file
git diff filename.txt
```


## 📋 Quick Command Reference

### Daily Commands
```bash
git status          # Check status
git add .           # Stage all files
git commit -m "msg" # Commit with message
git push            # Upload to GitHub
git pull            # Download from GitHub
```

### Branch Commands
```bash
git branch                    # List branches
git checkout -b new-branch   # Create branch
git checkout main           # Switch branch
git merge feature-branch    # Merge branch
```

### History Commands
```bash
git log --oneline    # View history
git reflog          # View all actions
git reset --hard    # Undo commits
git revert         # Reverse commit
```

### Emergency Commands
```bash
git stash              # Save work temporarily
git clean -fd          # Remove untracked files
git reset --hard HEAD  # Reset everything
git reflog            # Find lost commits
```


## 🎯 Complete Workflows

### New Project Workflow
```bash
# 1. Setup
cd ~/storage/downloads/my-project
git init && git add . && git commit -m "🎉 Initial commit"

# 2. Create GitHub repo and push
gh repo create my-project --public
git remote add origin https://github.com/username/my-project.git
git push -u origin main
```

### Daily Development
```bash
# 1. Start work
git pull origin main
git checkout -b feature/awesome-feature

# 2. Make changes and commit
git add . && git commit -m "✨ Add awesome feature"

# 3. Push and create PR
git push -u origin feature/awesome-feature
gh pr create --title "Add awesome feature"
```

### Fix Mistakes
```bash
# Undo last commit but keep changes
git reset --soft HEAD~1

# Completely undo last commit
git reset --hard HEAD~1

# Recover if you made mistake
git reflog && git checkout abc1234
```

## 📞 Support & Resources

Need help or want to connect with others?

👉 **Join our community:** [Debug Angels Telegram Group](https://t.me/DebugAngels)

## 📝 License

This project is **open source**.  
You're free to **use, modify, and distribute** it as needed.