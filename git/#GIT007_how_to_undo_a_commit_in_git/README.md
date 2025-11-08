## Method 1: Undo the Last Commit, Keep the Changes

This is the safest way to undo your *last local commit*. It's perfect for when you committed too early or forgot to include a file.

The `--soft` flag moves the `HEAD` pointer back one commit, effectively "undoing" the commit. However, it **keeps all your changes** from that commit in your staging area (as "Changes to be committed").

### **The Command**

```bash
# Move back 1 commit, keeping all changes staged
git reset --soft HEAD~1
```

### **Example:**

Imagine you just committed "Feat: Add login page", but forgot to add the CSS file.

```bash
# Step 1: Check the status (nothing to commit)
git status
# On branch feature/login
# nothing to commit, working tree clean

# Step 2: Undo the last commit, but keep the changes
git reset --soft HEAD~1

# Step 3: Check the status again
git status
# On branch feature/login
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#       modified:   index.html
#       new file:   login.js
```

Now, your files are back in the staging area. You can add the missing CSS file and make a new, complete commit.

---

## Method 2: Undo the Last Commit, Discard the Changes

This is a **destructive** and **irreversible** method. Use it only if you are absolutely sure you want to permanently delete the last commit *and* all the work in it.

The `--hard` flag moves the `HEAD` pointer back one commit, discards all changes from that commit in the staging area, **and** overwrites the files in your working directory to match the previous commit.

### **The Command**

```bash
# WARNING: This permanently deletes uncommitted changes!
# Move back 1 commit, destroying all changes
git reset --hard HEAD~1
```

### **Example:**

You just committed "Bad commit, this was a mistake".

```bash
# Step 1: Check the log (we see the bad commit)
git log -2 --pretty=%s
#   Bad commit, this was a mistake
#   Feat: Add login page

# Step 2: Destroy the last commit
git reset --hard HEAD~1

# Step 3: Check the log again
git log -1 --pretty=%s
#   Feat: Add login page

# Step 4: Check status
git status
# On branch feature/login
# nothing to commit, working tree clean
```

The bad commit is gone, and all the file changes associated with it have been completely erased from your project.

---

## Method 3: Undo a Specific Pushed Commit (The Safe Way)

If you have already **pushed a commit to a remote server** (or if it's an older commit you can't easily `reset`), you should **NOT** use `git reset`. Rewriting shared history is dangerous.

The safe way to "undo" a public commit is `git revert`. This command does **not** rewrite history. Instead, it creates a **new commit** that does the exact opposite of the commit you want to undo. If the old commit added a line, the new "revert" commit will remove that line.

### **The Command**

```bash
# Revert a specific commit by its hash
git revert <commit-hash>
```

### **Example:**

Imagine we want to undo the "Feat: Add buggy feature" commit, which is already on the remote.

```bash
# Step 1: Find the bad commit's hash
git log --oneline -3
#   c8d9e0f (HEAD -> main) Fix: Typo
#   a1b2c3d Feat: Add buggy feature
#   f4e5d6c Initial commit

# Step 2: Revert the bad commit
git revert a1b2c3d
# This opens an editor to confirm the new commit's message.
# (It's usually pre-filled with "Revert 'Feat: Add buggy feature'")
# Save and close the editor.

# Step 3: Check the log again
git log --oneline -3
#   e7f8g9h (HEAD -> main) Revert "Feat: Add buggy feature"
#   c8d9e0f Fix: Typo
#   a1b2c3d Feat: Add buggy feature
```

The bad feature is now "undone" by the new commit, and your history is safe to push without force.

---

## Method 4: Remove a Specific Unpushed Commit

If you want to completely **delete an older commit** that has **not been pushed** to a remote server, you can use interactive rebase.

This process lets you "go back in time" and remove a specific commit from the history. This is a **history rewriting** operation, so it should **never** be used on commits that others already have.

### **The Command**

```bash
# Start an interactive rebase from a commit *before* the one you want to delete.
git rebase -i <commit-hash-before-target>
# or
git rebase -i HEAD~4
```

### **The Process**

1.  Run the `git rebase -i` command.
2.  Your text editor will open with a list of commits.
3.  Find the line with the commit you want to remove.
4.  Change the word `pick` to `drop` (or `d`), or just **delete the entire line**.
5.  Save and close the file.

Git will then remove that commit from the history.

---

## Example: Removing a Commit with Rebase

Let's walk through the process. Imagine we want to remove the "WIP: Test commit" from our local history.

```bash
# Step 1: Check the Log
git log --oneline -3
#   c3a4b5d (HEAD -> main) Feat: Add login form
#   a2b3c4d WIP: Test commit 
#   f1e2d3c Initial project setup

# Step 2: Start the Rebase
# We must rebase from *before* the commit we want to drop (`a2b3c4d`). 
# We use the hash of the commit before it (`f1e2d3c`).
git rebase -i f1e2d3c

# Step 3: Editor (Choose Action)
# This editor opens. We change `pick` to `drop` on our target commit (or just delete the line).
pick a2b3c4d WIP: Test commit 
pick c3a4b5d Feat: Add login form
# We change 'pick' to 'drop'
drop a2b3c4d WIP: Test commit
pick c3a4b5d Feat: Add login form
# Save and close this file. The rebase completes.

# Step 4: Check Log (After Rebase)
git log --oneline -2
#   e7f8g9h (HEAD -> main) Feat: Add login form
#   f1e2d3c Initial project setup
```

The "WIP" commit has been completely erased from the history.
