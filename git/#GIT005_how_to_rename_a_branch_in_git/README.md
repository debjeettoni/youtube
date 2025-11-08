## How to rename current Local Branch

This is the most direct way to rename a branch. If you are already on the branch you want to rename, you can use the `-m` (for "move") flag with just the new name.

### **The Command**

```bash
git branch -m <new-name>
```

### **Example:**

Let's say we are on a branch called `feature/typo` and want to rename it.

```bash
# Step 1: Check our current branch
git branch
#   main
# * feature/typo

# Step 2: Rename the current branch
git branch -m feature/login

# Step 3: Check the branch list again
git branch
#   main
# * feature/login
```

The branch has been instantly renamed.

---

## How to rename another Local Branch

This is the safest method, as it ensures you are not actively working on the branch you are trying to change. You switch to a different branch (like `main`) and then rename the branch from there.

### **The Command**

```bash
git branch -m <old-name> <new-name>
```

### **Example:**

Let's rename `feature/login` to `feature/authentication`.

```bash
# Step 1: First, make sure you are NOT on the branch. We'll switch to main.
git switch main

# Step 2: Now, rename the branch
git branch -m feature/login feature/authentication

# Step 3: Check the branch list
git branch
#   feature/authentication
# * main
```

The branch has been successfully renamed.

---

## How to rename a Remote Branch

You cannot directly "rename" a remote branch. The process is a two-step "push and delete" operation:

### **The Command**

```bash
# Push your new local branch name to the remote
git push origin -u <new-name>

# Delete the old branch name from the remote
git push origin --delete <old-name>
```

### **Example:**

This assumes you have already renamed your local branch from `feature/login` to `feature/authentication`.

```bash
# Step 1: Push the new branch and set it to track
git push origin -u feature/authentication

# Step 2: Delete the old branch from the remote
git push origin --delete feature/login
```

Your branch is now successfully "renamed" on the remote server.

---

## How to Sync Branch Rename (For Teammates)

When you rename a remote branch, your teammates' local repositories are not automatically updated. They need to follow a 3-step process "fetch & prune, rename local, and set new upstream for local" to sync their local branch.

### **The Command**

```bash
# Prune remote-tracking branches 
git fetch origin --prune

# Rename Local Branch
git branch -m <old-name> <new-name>

# Set the New Upstream
git branch -u origin/<new-name> <new-name>
```

### **Example:**

Assuming their old local branch was named `feature/login`

```bash
# Step 1: Fetch & Prune 
git fetch origin --prune

# Step 2: Rename their Local Branch
git branch -m feature/login feature/authentication

# Step 3: Set the New Upstream
git branch -u origin/feature/authentication feature/authentication
```

Now, their local repository is fully in sync.

