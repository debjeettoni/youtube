## Method 1: Remove File from Git AND Disk

This is the most common way to remove a file. You use `git rm` when you want to **delete a file from your project** and also **tell Git to stop tracking it**.

This command does two things:

1.  It removes the file from your working directory (just like the normal `rm` command).
2.  It stages that removal, so the deletion will be part of your next commit.

### **The Command**

```bash
# This removes the file from the disk AND stages the deletion
git rm <path/to/file>

# Example
git rm config.log
```

### **Example:**

You have a temporary log file `config.log` that you accidentally added to Git, and you want to delete it completely.

```bash
# Step 1: Check the file's status (it's tracked)
git status
# On branch main
# nothing to commit, working tree clean

# Step 2: Run the git rm command
git rm config.log
# rm 'config.log'

# Step 3: Check the status again
git status
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#       deleted:    config.log
```

The file is now gone from your folder, and the deletion is staged. Just make a new commit to record this removal in your project's history.

---

## Method 2: Remove File from Git *Only* (Keep Local File)

This is a very common scenario. You use this when you accidentally committed a file (like a personal configuration file, a log, or an IDE setting) and you want Git to **stop tracking it**, but you **do not** want to delete the file from your computer.

The `--cached` flag tells Git to remove the file from the staging area (and tracking), but to *leave the file on your disk* untouched.

### **The Command**

```bash
# Untrack the file, but leave it on the disk
git rm --cached <path/to/file>

# Example
git rm --cached config.ini
```

### **Example:**

You accidentally committed `config.ini`, which contains personal settings.

```bash
# Step 1: Check the file system (the file is there)
ls
#   config.ini
#   README.md

# Step 2: Untrack the file
git rm --cached config.ini
# rm 'config.ini'

# Step 3: Check the status
git status
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#       deleted:    config.ini

# Step 4: Check the file system again (the file is STILL there)
ls
#   config.ini
#   README.md
```

The file is untracked and staged for deletion *from the repository*. After you commit this, you should immediately add `config.ini` to your `.gitignore` file to prevent it from being added again.

---

## Method 3: Remove File from *All* Past History

This is an advanced and **destructive** operation. You should only use this if you accidentally committed **sensitive data** (like a password, secret key, or large binary file) and you need to permanently erase it from your *entire* project history.

A simple `git rm` commit only removes the file *going forward*. The file still exists in all the old commits. This command will rewrite your entire history to remove the file from *every single commit*.

### **The Command**

```bash
# WARNING: This command is slow, complex, and rewrites your
# entire history. Back up your repository before running this.

git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch <path/to/your/file>" \
  --prune-empty --tag-name-filter cat -- --all
```

*Note: This command is very powerful. It's often recommended to use faster, simpler, external tools like the `BFG Repo-Cleaner` for this task.*

### **After Running the Command**

1.  The file will be gone from all commits.
2.  Your entire commit history will be rewritten (all commit hashes will change).
3.  You **must** force push to the remote to overwrite the old, bad history. This can be very disruptive to your teammates.

```bash
# You must force-push to update the remote
git push origin --force --all
git push origin --force --tags
```

