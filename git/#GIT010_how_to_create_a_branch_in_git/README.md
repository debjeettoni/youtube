## Method 1: Create a New Branch and Switch to It (The Common Way)

This is the most common and efficient way to create a branch. You use this when you're ready to start working on a new feature or bugfix *right now*.

This single command does two things at once:

1.  It **creates** the new branch.
2.  It **switches** your working directory to that new branch.

### **The Command**

```bash
# The modern, recommended command
git switch -c <new-branch-name>

# The classic command (does the same thing)
git checkout -b <new-branch-name>
```

### **Example:**

You are on the `main` branch and want to start a new feature called `feature/login`.

```bash
# Step 1: Check your current branch
git branch
# * main
#   develop

# Step 2: Create the new branch and switch to it
git switch -c feature/login

# Step 3: Check your branch list again
git branch
# * feature/login
#   main
#   develop
```

You are now on the `feature/login` branch and can start working immediately.

---

## Method 2: Create a New Branch (Without Switching)

This method is less common, but it's useful if you want to **create a new branch pointer** for later use, without leaving your current branch.

It simply creates the new branch name and points it to your current commit (`HEAD`), but it **does not** check it out. You stay on the branch you are currently on.

### **The Command**

```bash
# Just create the branch; don't switch to it
git branch <new-branch-name>
```

### **Example:**

You are on the `main` branch and want to create a `hotfix` branch, but you aren't ready to start working on it yet.

```bash
# Step 1: Check your current branch
git branch
# * main
#   develop

# Step 2: Create the new branch in the background
git branch hotfix/typo

# Step 3: Check your branch list again
git branch
#   develop
#   hotfix/typo
# * main
```

As you can see, the `hotfix/typo` branch was created, but you are still on the `main` branch.

---

## Method 3: Create a Branch from a Specific Tag

This is a very common and useful operation, especially for creating a patch or hotfix for a specific release version.

Since a tag is a permanent pointer to a specific commit, this command creates a new branch that starts its history from that *exact* commit.

### **The Command**

```bash
# Create a new branch *from* an existing tag
git branch <new-branch-name> <tag-name>

# To create and switch to it at the same time:
git switch -c <new-branch-name> <tag-name>
```

### **Example:**

You need to create a `hotfix/v1.0.1` branch to fix a bug in your `v1.0.0` release.

```bash
# Step 1: Check your current branch
git branch
# * main

# Step 2: Create the new hotfix branch from the old tag
# We'll also switch to it at the same time.
git switch -c hotfix/v1.0.1 v1.0.0

# Step 3: Check your new branch
git branch
# * hotfix/v1.0.1
#   main

# Step 4: Check your log
git log -1 --pretty=%s
# (HEAD -> hotfix/v1.0.1, tag: v1.0.0) Release 1.0.0
```

You are now on a new branch that starts *exactly* at the `v1.0.0` release commit, ready to make your fix.

---

## Method 4: Create a Branch from a Specific Commit

This method is very similar to creating from a tag, but it's more direct. You use this when you want to start a new line of work from *any* specific point in your project's history.

All you need is the **commit hash** (the unique ID) of the commit you want to branch from. This is perfect for investigating an old bug or resurrecting a deleted feature.

### **The Command**

```bash
# Create a new branch *from* a specific commit hash
git branch <new-branch-name> <commit-hash>

# To create and switch to it at the same time:
git switch -c <new-branch-name> <commit-hash>
```

### **Example:**

You find a bug and, using `git log`, you see it was introduced in commit `a1b2c3d`. You want to branch from the commit *before* it, `f4e5d6c`, to fix it.

```bash
# Step 1: Find the commit hash you want to start from
git log --oneline -3
#   a1b2c3d (HEAD -> main) Feat: Add buggy feature
#   f4e5d6c Feat: Add stable user profile
#   b7a8c9d Initial commit

# Step 2: Create the new branch from the "stable" commit
git switch -c fix/profile-bug f4e5d6c

# Step 3: Check your new branch
git branch
# * fix/profile-bug
#   main

# Step 4: Check your log
git log -1 --pretty=%s
# (HEAD -> fix/profile-bug) Feat: Add stable user profile
```

You are now on a new branch that starts exactly at commit `f4e5d6c`.

---

## Create a new branch in Remote repository from Local branch

By default, a branch you create is **only local**. It does not exist on the remote server (like GitHub or GitLab) until you explicitly push it.

To share your new branch with teammates or open a pull request, you need to push it to the remote. The first time you push, you should use the `-u` (or `--set-upstream`) flag to link your local branch to the new remote branch.

### **The Command**

```bash
# Push the branch and set the remote as its "upstream"
git push -u <remote-name> <branch-name>

# A common example
git push -u origin feature/login
```

### **Example:**

You have just finished your work on the local `feature/login` branch and want to push it to `origin`.

```bash
# Step 1: Check your local branches
git branch
# * feature/login
#   main

# Step 2: Push the branch to the remote
git push -u origin feature/login

# Git will show a message like this:
# Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/user/repo.git
#  * [new branch]      feature/login -> feature/login
# Branch 'feature/login' set up to track remote branch 'feature/login' from 'origin'.
```

Your branch now exists on the remote, and it is "tracking" the remote branch. This means in the future, you can just run `git pull` or `git push` without any extra arguments.