## How to Stage Changes in Git (`git add`)

### **Agenda**

* What is the Staging Area? (A Quick Recap)
* The `git add` Command: Your Gateway to Committing
* Basic Usage: Adding Single and Multiple Files
* The All and Update Flags (`-A` and `-u`)
* Advanced Staging: Adding Changes Interactively (`--patch`)
* Navigating the Interactive Patch
* Summary: Key Takeaways

----

## What is the Staging Area? (A Quick Recap)

Before a change is permanently saved in your repository, it must first be placed in a special preparation area called the **Staging Area** (also known as the "index").

### **The Git Workflow**
Think of it as a draft of your next commit. It lets you group related changes together.

`Working Directory` -> **`git add`** -> `Staging Area` -> **`git commit`** -> `Repository`

* **Working Directory**: Contains the files you are currently modifying.
* **Staging Area**: Contains the specific changes you have selected to be in the *next* commit.
* **Repository**: Contains the permanent history of all your commits.

The staging area gives you complete control over what you save, allowing you to craft clean, logical commits.

---

## The `git add` Command: Your Gateway to Committing

The `git add` command is the bridge between your **Working Directory** and the **Staging Area**. Its primary job is to tell Git which changes you want to include in your next commit.

### **What `git add` does:**
* It takes a snapshot of the changes in a file *at that moment* and adds it to the staging area.
* If you modify the file again *after* running `git add`, the new changes will not be staged until you run `git add` again.

It does **not** change your repository. No changes are saved permanently until you run `git commit`.

---

## Basic Usage: Adding a Single File

The most common use of `git add` is to stage a single file that you have created or modified.

### **The Workflow**

1.  **Modify a file**: Make changes in your working directory.
2.  **Check the status**: Use `git status` to see your changes.
3.  **Stage the file**: Use `git add <filename>` to stage it.
4.  **Check the status again**: See how the file has moved to the staging area.

```bash
# 1. Create a new file
echo "Hello World" > new-file.txt

# 2. Check the status
git status
# On branch main
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         new-file.txt

# 3. Stage the file
git add new-file.txt

# 4. Check the status again
git status
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   new-file.txt
```

---

## Adding Multiple Files: The Power of the Dot (`git add .`)

Typing each filename individually is inefficient when you have changed many files. A common shortcut is to use a dot (`.`) to add all new and modified files from the current directory and all subdirectories.

```bash
# 1. Create two new files
touch file1.txt file2.txt

# 2. Check the status
git status
# On branch main
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         file1.txt
#         file2.txt

# 3. Stage all new and modified files in the current directory
git add .

# 4. Check the status again
git status
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   file1.txt
#         new file:   file2.txt
```

---

## Adding Everything: The All Flag (`git add -A`)

What if you have also deleted files? The `git add .` command won't stage a file deletion. For that, you need the `--all` flag, or its shortcut, `-A`. This command stages **all** changes in the entire repository, including new, modified, and deleted files.

```bash
# 1. Create and commit a file so it's tracked
touch file-to-delete.txt
git add file-to-delete.txt
git commit -m "Add file for deletion test"

# 2. Now, delete that file and create a new one
rm file-to-delete.txt
touch new-file.txt

# 3. Check the status
git status
# On branch main
# Changes not staged for commit:
#   (use "git add/rm <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         deleted:    file-to-delete.txt
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         new-file.txt

# 4. Stage EVERYTHING (new and deleted files)
git add -A

# 5. Check the status again
git status
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         deleted:    file-to-delete.txt
#         new file:   new-file.txt
```

---

## Updating What's Tracked: The Update Flag (`git add -u`)

The `--update` flag (or its shortcut, `-u`) provides a more specific way to stage changes. It stages modifications and deletions for **already tracked files only**. It will completely ignore any new, untracked files you have created.

```bash
# 1. We have a tracked file from the last commit. Let's modify it.
echo "Some new content" >> file-to-delete.txt

# 2. Create a brand new, untracked file.
touch another-new-file.txt

# 3. Check the status
git status
# On branch main
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#         modified:   file-to-delete.txt
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         another-new-file.txt

# 4. Stage only the tracked file's changes
git add -u

# 5. Check the status again
git status
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         modified:   file-to-delete.txt
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         another-new-file.txt
```

---

## Advanced Staging: Adding Changes Interactively (`git add --patch`)

This is one of Git's most powerful features. The `--patch` flag (or `-p`) allows you to review your changes piece by piece and decide exactly what you want to stage. This is perfect for splitting a large change into several smaller, logical commits.

### **How it works:**

Git will show you each "hunk" (a block of changes) in your file and ask you what to do with it.

```bash
# 1. We have a file with some content that is already committed.
#    Let's add two different, unrelated changes to it.
echo "Adding a new feature line" >> tracked-file.txt
echo "Fixing an unrelated bug on another line" >> tracked-file.txt

# 2. Start the interactive staging session
git add --patch
```

**Example Output (The Prompt):**

```bash
diff --git a/tracked-file.txt b/tracked-file.txt
index e69de29..d803020 100644
--- a/tracked-file.txt
+++ b/tracked-file.txt
@@ -0,0 +1,2 @@
+Adding a new feature line
+Fixing an unrelated bug on another line
Stage this hunk [y,n,q,a,d,s,e,?]?
```

You are now in an interactive session, which we will learn how to navigate in the next slide.

---

## Navigating the Interactive Patch

When you are in the interactive session, Git presents you with a list of single-letter commands to decide the fate of each "hunk" of code.

### **Common Commands:**
You type one of these letters and press Enter at the prompt:
`Stage this hunk [y,n,q,a,d,s,e,?]?`

* **`y`** - Yes, stage this hunk.
* **`n`** - No, do not stage this hunk for now.
* **`s`** - **Split** the current hunk into smaller hunks. This is extremely useful if Git grouped unrelated changes together.
* **`q`** - Quit the interactive session. Any hunks you've already staged will remain staged.
* **`e`** - Manually **edit** the current hunk. This gives you line-by-line control but is a more advanced option.
* **`?`** - Print help to see what all the commands do.

---

## Summary: Key Takeaways

Let's quickly review the most important points about staging changes with `git add`.

* **It Populates the Staging Area**: `git add` is the command you use to move changes from your working directory to the staging area, preparing them for the next commit.
* **Stage What You Need**: You can stage a single file (`git add <file>`), or all changes in the current directory (`git add .`).
* **Use Flags for Control**:
    * `git add -A` stages **all** changes in the repository (new, modified, and deleted).
    * `git add -u` stages **updates** to tracked files only (modified and deleted).
* **Stage with Precision using Patch**: The `--patch` flag (`-p`) is a powerful tool that lets you review and stage changes interactively, piece by piece.