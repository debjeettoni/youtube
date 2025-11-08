## Method 1: Find a Deleted *Local* Branch (Using `git reflog`)

If you **force-deleted a local branch** (with `git branch -D`), you can almost always recover it using the `git reflog`.

The `reflog` is Git's private journal. It records exactly where your `HEAD` pointer (what you're working on) has been. Even if you delete a branch, the `reflog` still has a record of the last commit you were on *while you were on that branch*.

### **The Command**

```bash
# Show the reflog and look for the commit you were on
git reflog

# You can also search for the branch name
git reflog | grep <branch-name>
```

### **Example:**

You just force-deleted a branch called `feature/bad-idea`.

```bash
# Step 1: Search the reflog for the branch name
git reflog | grep "feature/bad-idea"

# Step 2: Analyze the output
# The output will show the last things you did on that branch
# b1a2c3d HEAD@{4}: checkout: moving from feature/bad-idea to main
# a9b8c7d HEAD@{5}: commit: Add broken code  <-- THIS IS IT

# The commit hash 'a9b8c7d' is the last commit from that branch.
# This is the "lost" commit you need.
```

Once you have this commit hash, you can easily re-create the branch from it.

---

## Method 2: Find a Deleted *Remote* Branch (From PRs or CI Logs)

The `reflog` is **local**; it only exists on the machine where the work was done. If a branch is deleted from the remote (e.g., after a pull request is merged) and you never had it locally, you **cannot** use your `reflog` to find it.

You must find the last commit hash from an external source. The two best places to look are:

*  **The Pull Request (PR):**
    * Go to the "Pull Requests" tab on GitHub or GitLab and look for the "merged" or "closed" PR associated with that branch.
    * The PR will show you the full list of commits and the final commit hash.

*  **Your CI (Continuous Integration) Tool:**
    * Tools like Jenkins, Travis CI, or GitHub Actions keep a log of every build.
    * Find the build that ran for that branch's last commit, and the log will show you the commit hash it used.

This commit hash is what you need to restore the branch.

---

## How to Re-create and Push the Restored Branch

Once you have the **commit hash** (either from `reflog` or a Pull Request), "restoring" the branch is just a two-step process:

1.  **Re-create** the branch locally, pointing to that exact commit.
2.  **Push** that new local branch back up to the remote server.

### **The Command**

```bash
# Step 1: Re-create the branch locally from the lost commit
git branch <branch-name> <commit-hash>

# Step 2: Push the new branch and set it to track
git push -u <remote-name> <branch-name>
```

### **Example:**

```bash
# Step 1: Check your branches (the branch is gone)
git branch
# * main
#   develop

# Step 2: Re-create the branch from the commit hash
git branch feature/login a9b8c7d

# Step 3: Check your branches again (it's back locally)
git branch
#   develop
#   feature/login
# * main

# Step 4: Push the restored branch back to the remote
git push -u origin feature/login
# Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/user/repo.git
#  * [new branch]      feature/login -> feature/login
```