## Method 1: Delete a Merged Local Branch (The Safe Way)

This is the standard, safe way to delete a local branch. You use this command *after* your feature branch has been successfully merged into your main branch (e.g., `main` or `develop`).

The lowercase `-d` flag is a safety check. It will **only** delete the branch if its work is already fully merged. If the branch has unmerged changes, Git will stop and warn you.

### **The Command**

```bash
# Make sure you are NOT on the branch you want to delete
git switch main

# Delete the merged local branch
git branch -d <branch-name>
```

### **Example:**

You just merged your `feature/login` branch into `main` and now you want to clean it up.

```bash
# Step 1: Switch off the branch you want to delete
git switch main
# Switched to branch 'main'

# Step 2: Delete the now-merged feature branch
git branch -d feature/login
# Deleted branch feature/login (was a1b2c3d).

# Step 3: Check your branch list
git branch
# * main
#   develop
```

The `feature/login` branch is now gone from your local repository.

---

## Method 2: *Force*-Delete an Unmerged Local Branch

Sometimes you start a feature, realize it's a mistake, and want to **delete the branch and all its unmerged commits**.

In this case, the safe `-d` flag won't work. You must use the **uppercase `-D` flag**, which stands for "force delete." This tells Git to delete the branch and all its work, even if it was never merged.

### **The Command**

```bash
# WARNING: This permanently deletes unmerged work.
# Make sure you are NOT on the branch you want to delete
git switch main

# Force-delete the local branch
git branch -D <branch-name>
```

### **Example:**

You have a branch `feature/bad-idea` with 3 commits that you have *not* merged, and you want to permanently delete it.

```bash
# Step 1: Switch off the branch
git switch main
# Switched to branch 'main'

# Step 2: Try to use the safe delete (it fails)
git branch -d feature/bad-idea
# error: The branch 'feature/bad-idea' is not fully merged.
# If you are sure you want to delete it, run 'git branch -D feature/bad-idea'.

# Step 3: Use the force-delete flag instead
git branch -D feature/bad-idea
# Deleted branch feature/bad-idea (was 3f4g5h6).
```

The `feature/bad-idea` branch and its unmerged commits are now gone from your local repository.

---

## Method 3: Delete a Remote Branch

Deleting a local branch (with `-d` or `-D`) does **not** delete the branch from the remote server (like GitHub). To delete the branch from the remote, you must send a "delete" command to the remote.

This is commonly done after you have merged a pull request and deleted your local branch, and you now want to clean up the remote repository.

### **The Command**

```bash
# The modern, recommended command
git push <remote-name> --delete <branch-name>

# A common example
git push origin --delete feature/login
```

### **Example:**

Your `feature/login` branch has been merged on GitHub, and you've already deleted it locally. Now you want to remove it from the `origin` remote.

```bash
# Step 1: Run the remote delete command
git push origin --delete feature/login

# Git will show a message like this:
# To https://github.com/user/repo.git
#  - [deleted]         feature/login
```

The `feature/login` branch is now successfully deleted from the remote repository.

---

## Method 4: How Teammates Can Sync (Pruning)

When you delete a remote branch, your teammates' local repositories don't know about it yet. Their local Git will still show `origin/feature/login` as if it exists.

To clean up these "stale" branches, teammates can run a **fetch with the `--prune` flag**. This tells Git to check the remote server and remove any local references to remote branches that no longer exist.

### **The Command**

```bash
# This is the recommended command to run periodically
git fetch --prune

# You can also set this to happen automatically
git config --global fetch.prune true
```

### **Example:**

A teammate runs `git branch -r` and still sees the old branch.

```bash
# Step 1: Check remote branches (shows the old branch)
git branch -r
#   origin/main
#   origin/develop
#   origin/feature/login  <- This is stale

# Step 2: Run fetch with prune
git fetch --prune
# From https://github.com/user/repo.git
#  - [deleted]         (none)     -> origin/feature/login

# Step 3: Check remote branches again (it's gone)
git branch -r
#   origin/main
#   origin/develop
```

Their local repository is now fully in sync with the remote, and the stale branch reference is gone.