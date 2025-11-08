## What is a Git Tag? (Annotated vs. Lightweight)

A **Git Tag** is a pointer that marks a specific, important commit in your project's history. It's like a permanent "bookmark" for a commit, most commonly used to mark release versions (e.g., `v1.0.0`, `v2.1-beta`).

There are two types of tags in Git:

1.  **Annotated Tags** (Recommended)
    * These are stored as full objects in the Git database.
    * They are "annotated," meaning they include the tagger's name, email, the date, and a **tagging message** (just like a commit message).
    * They can also be GPG-signed for security.
    * **Best for:** All public releases and important milestones.

2.  **Lightweight Tags**
    * These are simple, "lightweight" pointers. They are just a name and a pointer to a commit.
    * They **do not** store any extra information like the tagger's name, date, or a message.
    * **Best for:** Temporary, private, or internal use.

For all serious work, you should **always use annotated tags**.

---

## Method 1: Create an Annotated Tag (The Recommended Way)

An **annotated tag** is the standard, recommended way to tag a commit. It stores extra metadata, including the tagger's name, email, the date, and a **tagging message**. This is essential for public releases and important milestones as it provides a full changelog or description for the tag.

### **The Command**

```bash
# -a stands for "annotated"
# This will open your default text editor to write a message:
git tag -a <tag-name>

# OR: Provide the message inline with the -m flag
git tag -a <tag-name> -m "Your tag message"
```

### **Example:**

You want to tag the current commit as `v1.0.0`.

```bash
# Step 1: Create an annotated tag with an inline message
git tag -a v1.0.0 -m "Initial release version 1.0.0"

# Step 2: Check your tags
git tag
#   v1.0.0

# Step 3: Show the tag's information
git show v1.0.0
# tag v1.0.0
# Tagger: Your Name <your.email@example.com>
# Date:   Thu Oct 30 20:30:00 2025 +0530
#
# Initial release version 1.0.0
#
# commit a1b2c3d4e5f6... (HEAD -> main)
# Author: Your Name <your.email@example.com>
# Date:   Thu Oct 30 20:25:00 2025 +0530
#
#   Feat: Add login page
```

---

## Method 2: Create a Lightweight Tag

A **lightweight tag** is the simplest way to tag a commit. It is nothing more than a name that points to a specific commit hash.

It does **not** store any extra information like the tagger's name, email, date, or a tagging message. These are best used for your own private, temporary markers.

### **The Command**

```bash
# Just provide a tag name and no other flags (like -a or -m)
git tag <tag-name>
```

### **Example:**

You want to create a quick, private tag called `temp-marker`.

```bash
# Step 1: Create the lightweight tag
git tag temp-marker

# Step 2: Check your tags
git tag
#   temp-marker
#   v1.0.0

# Step 3: Show the tag's information (compare to annotated)
git show temp-marker
# commit a1b2c3d4e5f6... (HEAD -> main, tag: temp-marker, tag: v1.0.0)
# Author: Your Name <your.email@example.com>
# Date:   Thu Oct 30 20:25:00 2025 +0530
#
#   Feat: Add login page
```

Notice that the `git show` command shows **no tag information**. It just shows the commit that the tag points to. This is the key difference.

---

## How to Tag an Older Commit

By default, the `git tag` command will tag your most recent commit (known as `HEAD`). However, you can easily tag **any commit in your history** by providing its commit hash (the unique ID) at the end of the command.

This is extremely useful for tagging a past release that you forgot to tag at the time.

### **The Command**

```bash
# Append the commit hash to the end of the command
git tag -a <tag-name> -m "Tag message" <commit-hash>
```

### **Example:**

You want to tag `v1.0.0` on a commit from last week, `a1b2c3d`.

```bash
# Step 1: Find the hash of the commit you want to tag
git log --oneline -3
#   c8d9e0f (HEAD -> main) Feat: Add new logo
#   f4e5d6c Fix: Login button style
#   a1b2c3d Feat: Add login page (this is the one we want)

# Step 2: Create the annotated tag on that specific commit
git tag -a v1.0.0 -m "Initial release 1.0.0" a1b2c3d

# Step 3: Verify by showing the tag
git show v1.0.0
# tag v1.0.0
# Tagger: Your Name <your.email@example.com>
# Date:   Thu Oct 30 21:00:00 2025 +0530
#
# Initial release 1.0.0
#
# commit a1b2c3d4e5f6...  <-- Note: It's pointing to the old commit
# Author: Your Name <your.email@example.com>
# Date:   Wed Oct 22 14:00:00 2025 +0530
#
#   Feat: Add login page
```

---

## How to Push Tags to a Remote Server

By default, when you run `git push`, it does **not** send your tags to the remote server. Tags are considered "private" to your local repository until you explicitly push them.

You must push tags in a separate step. You can either push one specific tag at a time or push all of your local tags at once.

### **The Command**

```bash
# Method 1: Push a single, specific tag
git push <remote-name> <tag-name>

# Method 2: Push ALL local tags
# (Use this with caution, make sure you don't have temporary tags)
git push <remote-name> --tags
```

### **Example:**

You have just created `v1.0.0` and `temp-marker` locally and want to push the official `v1.0.0` tag.

```bash
# Step 1: Push only the v1.0.0 tag to origin
git push origin v1.0.0

# Git will show a message like this:
# Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/user/repo.git
#  * [new tag]         v1.0.0 -> v1.0.0

# --- OR ---

# Step 2 (Alternative): Push all local tags
git push origin --tags

# Git will show:
# Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/user/repo.git
#  * [new tag]         temp-marker -> temp-marker
#  * [new tag]         v1.0.0 -> v1.0.0
```