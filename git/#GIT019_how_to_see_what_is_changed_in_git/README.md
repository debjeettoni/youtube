## How to See Unstaged Changes (In Your Working Directory)

This is the most common command for seeing what you are *currently* working on.

`git diff` shows you the difference between your **working directory** (the files you are actively editing) and the **staging area** (what you last ran `git add` on).

In simple terms: it shows all of your changes that you have **not yet staged**.

### **The Command**

```bash
# Show changes in your working directory that are not staged
git diff
```

### **Example:**

You modified `index.html` but have *not* run `git add` on it yet.

```bash
# Check your status
git status
# On branch main
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#         modified:   index.html

# Run git diff to see the changes
git diff
# diff --git a/index.html b/index.html
# index a1b2c3d..e4f5g6h 100644
# --- a/index.html
# +++ b/index.html
# @@ -1,3 +1,4 @@
#  <html>
#    <head><title>My App</title></head>
# -  <body></body>
# +  <body>
# +    <h1>Welcome</h1>
# +  </body>
#  </html>
```

This shows you removed the line `<body></body>` (marked with `-`) and added three new lines (marked with `+`).

---

## How to See Staged Changes (In Your Staging Area)

After you run `git add` on a file, its changes are moved to the **staging area**. At this point, the normal `git diff` command will show nothing, because your working directory *matches* your staging area.

To see the changes you have **already staged** (the changes that are *about to be committed*), you must use the `--staged` (or `--cached`) flag. This command shows the difference between your **staging area** and your **last commit** (HEAD).

### **The Command**

```bash
# Show changes that are staged (in the index)
git diff --staged
```

### **Example:**

```bash
# Check your status
git status
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         modified:   index.html
# 
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#         modified:   index.html

# Shows changes in the stage
git diff --staged
# diff --git a/index.html b/index.html
# --- a/index.html
# +++ b/index.html
# @@ -1,3 +1,4 @@
#  <html>
#    <head><title>My App</title></head>
# -  <body></body>
# +  <body>
# +    <h1>Welcome</h1>
# +  </body>
#  </html>
```

This shows the `h1` addition is staged and ready to be committed, while the new comment is still unstaged.

---

## How to See Changes in a Specific Commit (`git show`)

If you want to see what changed in *one specific commit* (not the difference between two), the easiest command is `git show`.

This command shows you two things for a single commit:

1.  The full **commit message** (author, date, and message).
2.  The full **diff** (the "patch") of what changed in that commit.

By default, `git show` will display the most recent commit (`HEAD`).

### **The Command**

```bash
# Show the changes from the *most recent* commit (HEAD)
git show

# Show the changes from a specific commit hash
git show <commit-hash>
```

### **Example:**

You want to see the details for an older commit, `a1b2c3d`.

```bash
git show a1b2c3d
# commit a1b2c3d4e5f6...
# Author: Your Name <your.email@example.com>
# Date:   Thu Oct 30 20:30:00 2025 +0530
# 
#     Feat: Add login page
# 
# diff --git a/index.html b/index.html
# index e69de29..b546a1e 100644
# --- a/index.html
# +++ b/index.html
# @@ -1,1 +1,5 @@
#  <html>
# +  <body>
# +    <form>Login</form>
# +  </body>
#  </html>
```

---

## How to See Changes Between Any Two Points (Commits, Branches, or Tags)

To see the *total difference* between any two points in your project's history, you use the `git diff` command.

This is an extremely powerful command. In Git, a **branch name**, a **tag name**, and a **commit hash** are all just "references" that point to a specific commit. You can use any of them with `git diff` to compare them.

This command will show you a combined "patch" of all the changes between the two points.

### **The Command**

```bash
# The general syntax
git diff <reference-1> <reference-2>
```

### **Common Examples:**

```bash
# 1. Between two specific commit hashes
git diff a1b2c3d e4f5g6h

# 2. Between two tags
git diff v1.0.0 v1.1.0

# 3. Between two local branches
git diff main feature/login

# 4. Between a tag and a branch
git diff v1.0.0 main

# 5. Between your local branch and the remote branch
# (This is great for seeing what work you have that's not pushed)
git diff origin/main main

# 6. Between a base branch and feature branch (three-dot diff)
# (Shows only the changes on your feature branch, like a pull request)
git diff main...feature/login
```

---

## How to See a *Summary* of Changes in the Log (`git log --stat`)

Sometimes you don't need to see the *exact* line-by-line changes (like with `git log -p`). You just want a high-level **summary** of *what files* were changed in each commit and *how much* they changed.

The `--stat` flag is perfect for this. It appends a small "statistics" summary to each commit, showing a list of files, how many lines were added/removed, and a visual summary of the changes.

### **The Command**

```bash
# Show the log with a summary of changes for each commit
git log --stat

# This is extremely powerful when combined with --oneline
git log --oneline --stat
```

### **Example Output (`git log --stat -1`):**

```bash
$ git log --stat -1

commit a1b2c3d4e5f6...
Author: Your Name <your.email@example.com>
Date:   Thu Oct 30 20:30:00 2025 +0530

    Feat: Add login page

 index.html | 4 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
```

This summary clearly shows that the "Feat: Add login page" commit changed 1 file (`index.html`), adding 4 lines and removing 1 line.

### **Example Output (`git log --oneline --stat -1`):**

```bash
$ git log --oneline --stat -1

a1b2c3d (HEAD -> main) Feat: Add login page
 index.html | 4 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
```

---