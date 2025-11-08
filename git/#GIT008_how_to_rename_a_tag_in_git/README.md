## Renaming a Tag That is Only Local (Not Pushed)

If you have created a tag but **not yet pushed it** to the remote server, the process is very simple. It's just two steps: create the new tag, and delete the old one.

This is the safest scenario, as it only affects your local repository.

### **The Command**

```bash
# 1. Create the new tag, pointing to the same commit as the old tag
git tag <new-name> <old-name>

# 2. Delete the old tag
git tag -d <old-name>
```

*Note: The `git tag <new-name> <old-name>` command works because `old-name` resolves to the commit hash it points to.*

### **Example:**

You just tagged a commit as `v1.0-typo` but meant `v1.0`.

```bash
# Step 1: Check the commit the old tag points to
git rev-parse v1.0-typo
# a1b2c3d4e5f6...

# Step 2: Create the new tag pointing to that same commit
git tag v1.0 v1.0-typo

# Step 3: Delete the old, incorrect tag
git tag -d v1.0-typo

# Step 4: Verify
git tag
#   v1.0
```

Your tag is now successfully renamed locally.

---

## The Full Process: Renaming a Tag That is Already Pushed

If the tag is already on a remote server (like GitHub), you must perform a 4-step process: rename locally, then push the new tag, and finally, delete the old tag from the remote.

This is necessary because you need to update both your local and remote repositories.

### **The Commands**

```bash
# Step 1: Rename the tag locally (as before)
git tag <new-name> <old-name>
git tag -d <old-name>

# Step 2: Push the new tag to the remote
git push origin <new-name>

# Step 3: Delete the old tag from the remote
git push origin --delete <old-name>
```

### **Example:**

We want to rename `v1.0-typo` to `v1.0`, which is already on `origin`.

```bash
# Step 1: Rename locally
git tag v1.0 v1.0-typo
git tag -d v1.0-typo

# Step 2: Push the new, correct tag
git push origin v1.0

# Step 3: Delete the old, incorrect tag from the remote
git push origin --delete v1.0-typo
```

The tag is now successfully "renamed" on the remote server.

---

## How Teammates Can Sync the Renamed Tag

When you rename a tag on the remote, your teammates' local repositories are not automatically updated. They still have the old, incorrect tag.

They must manually **fetch** the changes (including the "delete" instruction) and then **delete** their old local tag.

### **The Command**

```bash
# Step 1: Fetch all tags, and use --prune-tags to remove
# remote-tracking tags that no longer exist on the remote.
git fetch origin --prune-tags

# Step 2: (Optional but Recommended)
# Manually delete the old local tag to avoid confusion.
git tag -d <old-name>
```

### **Example:**

A teammate needs to sync the `v1.0-typo` -\> `v1.0` rename.

```bash
# Step 1: They fetch from the remote with pruning
# This downloads the new "v1.0" tag and removes the
# remote reference to the deleted "v1.0-typo" tag.
git fetch origin --prune-tags

# Step 2: They still have the old local tag.
# They should delete it manually.
git tag -d v1.0-typo

# Step 3: Now their local tag list is clean and correct
git tag
#   v1.0
```

Their local repository is now fully in sync with the remote.