## Method 1: Delete a Local Tag

This is the first step in deleting a tag. This command removes the tag from your **local repository only**.

It does **not** affect the remote server. If you have already pushed the tag, you will also need to delete it from the remote, which we will cover in the next slide.

### **The Command**

```bash
# -d stands for "delete"
git tag -d <tag-name>
```

### **Example:**

You have a local tag `v1.0-typo` that you want to remove.

```bash
# Step 1: List your current local tags
git tag
#   v1.0
#   v1.0-typo

# Step 2: Delete the incorrect tag
git tag -d v1.0-typo
# Deleted tag 'v1.0-typo' (was a1b2c3d)

# Step 3: List your tags again
git tag
#   v1.0
```

The `v1.0-typo` tag is now gone from your local repository.

---

## Method 2: Delete a Remote Tag

Deleting a tag locally (with `git tag -d`) does **not** remove the tag from the remote server. To delete the tag from the remote, you must explicitly "push" a delete instruction to the remote repository.

This command tells the remote server (like `origin`) to remove the tag pointer.

### **The Command**

```bash
# Push a "delete" command for a specific tag
git push <remote-name> --delete <tag-name>

# A common example
git push origin --delete v1.0-typo
```

### **Example:**

You have deleted `v1.0-typo` locally, but it still exists on `origin`.

```bash
# Step 1: Run the remote delete command
git push origin --delete v1.0-typo

# Git will show a confirmation message:
# To https://github.com/user/repo.git
#  - [deleted]         v1.0-typo
```

The `v1.0-typo` tag is now successfully deleted from the remote server.

---

## Method 3: How Teammates Can Sync (Pruning Tags)

When you delete a tag from the remote server, your teammates' local repositories are not automatically updated. They still have the old, "stale" tag on their local machines.

To clean up these stale tags, teammates can run a **fetch with the `--prune-tags` flag**. This tells Git to check the remote server and remove any local tags that no longer exist on the remote.

### **The Command**

```bash
# Fetch from origin and remove any stale tags
git fetch origin --prune-tags
```

### **Example:**

A teammate still has the `v1.0-typo` tag locally, even after you deleted it from the remote.

```bash
# Step 1: They list their tags (the old tag is still there)
git tag
#   v1.0
#   v1.0-typo  <- Stale local tag

# Step 2: They fetch from the remote with pruning
git fetch origin --prune-tags
# From https://github.com/user/repo.git
#  - [deleted]         (none)     -> v1.0-typo

# Step 3: They list their tags again (it's gone)
git tag
#   v1.0
```

Their local repository is now fully in sync with the remote, and the stale tag is gone.

