## How to change the Last Commit Message

This is the simplest case. If you just made a commit and immediately realize you made a typo or want to rephrase the message, you can use the `--amend` flag. This command replaces the previous commit with a new one that has your corrected message.

### **The Command**

```bash
# This will open your default text editor to change the message
git commit --amend

# OR: Provide the new message inline
git commit --amend -m "Your new, correct message"
```

### **Example:**

Let's say we just committed "Feat: Add login form typo".

```bash
# Step 1: Check the last commit message
git log -1 --pretty=%s
#   Feat: Add login form typo

# Step 2: Amend the message directly
git commit --amend -m "Feat: Add login form"

# Step 3: Check the log again
git log -1 --pretty=%s
#   Feat: Add login form
```

The old commit has been replaced with the new, corrected one.

---

## How to change an Older Commit Message

To change a message for an older commit (not the very last one), you must use **interactive rebase**.

This process lets you "go back in time" to a point before the commit you want to change, and then tell Git which commits to edit.

### **The Command**

```bash
# Start an interactive rebase from a commit before the one you want to edit.
# You can use its hash, or 'HEAD~N' to go back N commits.
git rebase -i <commit-hash-before-target>
# or
git rebase -i HEAD~3
```

### **The Process**

1.  Run the `git rebase -i` command.
2.  Your text editor will open with a list of commits.
3.  Find the commit message you want to change, and change the word `pick` to `reword` (or just `r`).
4.  Save and close that file.
5.  Git will then stop at that commit and open a new editor, allowing you to type the new message.
6.  Save and close that second editor to finish.

---

## Example: Interactive Rebase (Reword)

Let's walk through the process. Imagine our log looks like this, and we want to fix "Add user profile typo".

```bash
# Step 1: Check the Log
git log --oneline -3
# c3a4b5d (HEAD -> main) Feat: Add login form
# a2b3c4d Feat: Add user profile typo
# f1e2d3c Initial project setup

# Step 2: Start the Rebase
# We must start the rebase from *before* the commit we want to edit (target: `a2b3c4d`).
# So, we use the hash of the commit before it (`f1e2d3c`).
git rebase -i f1e2d3c

# Step 3: Editor 1 (Choose Action)
# This editor opens. We change `pick` to `reword` on our target commit.
# Change 'pick' to 'reword' on the line you want to edit
# reword a2b3c4d Feat: Add user profile typo
# pick c3a4b5d Feat: Add login form
# Save and close this file.

# Step 4: Editor 2 (Change Message)
# Git immediately opens a second editor. We fix the message here.
# Change the message to be correct
# Feat: Add user profile
# Save and close this file. The rebase completes, and the message is fixed.
```

---

## Pushing a Rewritten Commit

Changing a commit message (with `amend` or `rebase`) **rewrites Git history**. This creates new commit IDs.

If you have already pushed the original commit to a remote server (like GitHub), Git will reject a normal `git push`. You must **force push** to update the remote branch with your new, corrected history.

### The Command (Use With Caution!)

```bash
# This is the safer option:
# It only forces the push if no one else has added new commits.
git push --force-with-lease

# This is the more dangerous option:
# It overwrites the remote branch no matter what.
git push --force
```

### ** IMPORTANT WARNING **

You should **NEVER** force push to a shared branch (like `main` or `develop`).

Force pushing rewrites the history that your teammates might already have. This can cause major conflicts and problems for them. Only force push if you are 100% sure you are the **only** person working on that branch.

