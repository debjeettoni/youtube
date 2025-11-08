## How to Restore a Single File from a Specific Commit

This is the most common and safest way to "restore" a file. You use this when you've made changes to a file, but you realize the version in an older commit was correct.

This command reaches into an old commit, grabs the version of the file as it existed *then*, and replaces the current file in your working directory with that old version. It uses the `git restore` command.

### **The Command**

```bash
# Get the file from a specific commit hash
git restore --source=<commit-hash> <path/to/file>
```

### **Example:**

Imagine you've made bad changes to `styles.css` and you want the version from the commit `a1b2c3d`.

```bash
# Step 1: Check the file (it's full of bad changes)
git status
# On branch main
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#       modified:   src/styles.css

# Step 2: Restore the file from the old commit
git restore --source=a1b2c3d src/styles.css

# Step 3: Check the status again
git status
# On branch main
# nothing to commit, working tree clean
```

The `src/styles.css` file in your working directory is now an exact copy of how it looked in commit `a1b2c3d`.

---

## How to Restore All Files to Match a Specific Commit (Viewing History)

This method is for **inspection and viewing only**. It lets you temporarily go back in time to see the *entire state* of your project as it was at a specific commit.

This command puts you in a "detached HEAD" state, meaning you are no longer on a branch. You are just "looking" at an old commit. This is **non-destructive** and perfect for seeing what the project looked like at that time.

### **The Command**

```bash
# Check out the entire project at that commit's state
git checkout <commit-hash>

# After you are done looking, check back out to your branch
git checkout main
```

### **Example:**

You want to see what the project looked like at commit `a1b2c3d`.

```bash
# Step 1: Check your current branch
git branch
# * main

# Step 2: "Go back in time" to the old commit
git checkout a1b2c3d
# Note: You are in 'detached HEAD' state...

# Step 3: Look around. All your files are now as they were at that commit.
# (Your newer files/commits seem to have "disappeared" - they are safe!)

# Step 4: When finished, go back to your branch
git checkout main
```

All your modern files and commits will instantly reappear, and you will be back on your `main` branch.

---

## How to Restore All Files to Match a Specific Commit (Rewriting History)

This is a **destructive** and **history-rewriting** command. Use it only when you want to permanently throw away all commits made *after* a specific point and make that old commit the new "end" of your branch.

This command is like a more powerful `git reset --hard HEAD~1`. Instead of just going back one commit, it goes back to a *specific* commit and deletes **everything** after it.

### **The Command**

```bash
# WARNING: This permanently deletes all commits after the hash!
# Force the current branch to be at this exact commit
git reset --hard <commit-hash>
```

### **Example:**

Your log shows commits A, B, C, and D. You decide that C and D were huge mistakes and you want to go back to commit B as if C and D never happened.

```bash
# Step 1: Check the log (we see the bad commits)
git log --oneline -4
#   d4d4d4d (HEAD -> main) Feat: Add buggy feature D
#   c3c3c3c Feat: Add broken feature C
#   b2b2b2b Feat: Add good feature B
#   a1a1a1a Initial commit

# Step 2: Reset the branch back to commit B
git reset --hard b2b2b2b

# Step 3: Check the log again
git log --oneline -1
#   b2b2b2b (HEAD -> main) Feat: Add good feature B
```

Commits C and D are now gone from your local branch history. This is **not** safe to do if you have already pushed commits C and D.