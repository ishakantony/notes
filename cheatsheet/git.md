# GIT - CHEATSHEET

### Basic Usage

```bash
# See current branch status
git status

# See list of branch (local and remote)
git branch -a

# Switch to a different branch (branch must exist)
git checkout [branch_name]

# Switch to a different branch (branch don't exist in local, will create automatically)
git checkout -b [branch_name]

# Switch to a different branch and track it (will create branch)
git checkout --track [remote_name]/[branch_name]

# Create new branch
git branch [branch_name]

# Delete local branch
git branch -d [branch_name]

# Delete remote branch
git push [remote_name] --delete [branch_name]

# Sync info between local and remote (--prune will delete list of remote branches if applicable)
git fetch --prune

# Stage changes, use "." to add all
git add [filename] [filename]
git add .

# Revert changes, use "." to add all
git checkout -- [filename]
git checkout -- .

# Commit staged changes, if you don't specify -m, git will prompt you for commit message
git commit -m "[commit_message]"

# You can combine staging and committing files with -am (no need to do git add) but it will not stage untracked files
git commit -am "[commit_message]"

# Modify last commit message, before push
git commit --amend

# Push commit to remote
git push

# View log
git log
```

### SCENARIO: Undo commit

```bash
# This will make all the committed files staged, as in before commit
git reset --soft HEAD~1
git reset --soft [commit_id]

# This will undo all changes of the committed files and delete untracked files
git reset --hard HEAD~1
git reset --hard [commit_id]
```

### SCENARIO: Git to remember credential

```bash
# do git pull and then enter username, password, it will remember it, try git pull again, won't ask for username/password
git config --global credential.helper store
```

### SCENARIO: Remove untracked files

```bash
# See how many files will be removed
git clean -n

# Remove files
git clean -f

# Remove directories
git clean -d

# Remove ignored files
git clean -fX

# Remove ignored and non-ignored files
git clean -fx
```

### SCENARIO: Rewrite History - Change Commit Message

```bash
# Change commit message until the beginning of history
# Change pick to reword
# save file, it will auto open new file to change every commits selected
git rebase -i --root

# Change commit message X commits behind HEAD
git rebase -i HEAD~[number]

# Rebase to specific commit [bbc643cd] is commid_id, don't forget the ^
git rebase -i 'bbc643cd^'
```

### SCENARIO: Advance logging

```bash
# View commits messages for every contributor
git shortlog

# View commits by author
git shortlog --author="[name]"

# View log tree
git log --oneline --all --graph --decorate

# Show files for commits and ranges
git show --stat --oneline HEAD
git show --stat --oneline b24f5fb
git show --stat --oneline HEAD^^..HEAD

# Show files with status
git log --name-status

# Range logging, see comits from master to feature (how further feature has evolve from master)
git log master..feature

# Find commit(s) that has specific content
git log -S"Hello, World!"

# Find all commits by author, can use regex
git log --oneline --author="ishak" --pretty=format:"%C(auto) %h %ae %d %s"

```

### SCENARIO: Delete Files

```bash
# Remove file in local and remote
git rm file1.txt
git commit -m "remove file1.txt"
git push

# Remove directory in local and remote
git rm -r path/to/directory
git commit -m "remove path/to/directory"
git push

# Remove files / directory(use -r) in REMOTE ONLY
git rm --cached file1.txt
git commit -m "remove file1.txt"
git push
```

### SCENARIO: Configure Custom Merge Tool

```bash
# I recommend to use meld
# Add the following to your .gitconfig file.
[diff]
    tool = meld
[difftool]
    prompt = false
[difftool "meld"]
    cmd = meld "$LOCAL" "$REMOTE"

# Normal diff will work as usual
git diff [filename]
# If you want to use the merge tool
git difftool [filename]

# Add the following to your .gitconfig file. (Recommend to use $BASE)
[merge]
    tool = meld
[mergetool "meld"]
    # Choose one of these 2 lines (not both!) explained below.
    cmd = meld "$LOCAL" "$MERGED" "$REMOTE" --output "$MERGED"
    cmd = meld "$LOCAL" "$BASE" "$REMOTE" --output "$MERGED"

# To use the merge tool, do git merge first, if got conflict only use mergetool
git merge [branch_to_merge]
git mergetool
```

### SCENARIO: Delete Branch (Local & Remote)

```bash
# Follow the steps below to rename a Local and Remote Git Branch:

# Start by switching to the local branch which you want to rename:
git checkout <old_name>

# Rename the local branch by typing:
git branch -m <new_name>

# At this point, you have renamed the local branch.
# If you’ve already pushed the <old_name> branch to the remote repository, perform the next steps to rename the remote branch.
# Push the <new_name> local branch and reset the upstream branch:
git push origin -u <new_name>

# Delete the <old_name> remote branch:
git push origin --delete <old_name>

# That’s it. You have successfully renamed the local and remote Git branch

# Delete local branches that has been merged (you must be in dev, or other branches that is your main)
git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d
```

### SCENARIO: Fix Dangling Branch

```bash
git remote set-head origin some_branch
```

### SCENARIO: Sync local tags with remote tags

```bash
git tag -l | xargs git tag -d
git fetch --tags
```
