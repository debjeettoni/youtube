## How to Find the "Lost" Commit (Using `git reflog`)

You cannot "undelete" a tag. A tag is just a pointer, and when you delete it, the pointer is gone.

To "restore" a deleted tag, you must first find the **exact commit hash** it used to point to. The easiest way to find this information (if you deleted it recently) is to search Git's "reference log," or `reflog`.

The `reflog` is a record of all the recent movements of `HEAD`, including checkouts, commits, and tag deletions.

### **The Command**

```bash
# Search the reflog for your tag's name
git reflog | grep <tag-name>
```

### **Example:**

You just deleted a tag called `v1.0.1` and you want to find its commit.

```bash
# Step 1: Search the reflog for "v1.0.1"
git reflog | grep v1.0.1

# Step 2: Analyze the output
# The output will look something like this:
# a1b2c3d HEAD@{1}: tag: deleting v1.0.1
```

The commit hash `a1b2c3d` is the commit your tag was pointing to. This is the "lost" commit you need.

---

## How to Re-create the Tag Locally

Once you have the commit hash from the `reflog`, "restoring" the tag is as simple as **re-creating it** using the exact same name and pointing it to that specific commit.

If it was an important release tag, you should re-create it as an **annotated tag** and include the original tag message if you remember it.

### **The Command**

```bash
# Use the hash you found to create the tag again
git tag -a <tag-name> -m "Tag message" <commit-hash>

# If it was a lightweight tag, just do:
git tag <tag-name> <commit-hash>
```

### **Example:**

From the `reflog`, we found our `v1.0.1` tag pointed to commit `a1b2c3d`.

```bash
# Step 1: Check your tags (the tag is gone)
git tag
#   v1.0.0

# Step 2: Re-create the tag using the hash we found
git tag -a v1.0.1 -m "Hotfix for login issue" a1b2c3d

# Step 3: Check your tags again
git tag
#   v1.0.0
#   v1.0.1
```

The tag is now successfully restored on your local repository, pointing to the correct, original commit.

---

## How to Push the Restored Tag to the Remote

Re-creating the tag locally does not automatically restore it on the remote server. If the tag was previously deleted from the remote (using `git push origin --delete <tag-name>`), you must now **push it back up**.

This is the final step to ensure the tag is fully restored for you and your entire team.

### **The Command**

```bash
# Push the single, restored tag back to the remote
git push <remote-name> <tag-name>
```

### **Example:**

You have just re-created `v1.0.1` locally, and now you need to push it back to the `origin` remote.

```bash
# Step 1: Push the restored tag
git push origin v1.0.1

# Git will show a confirmation message:
# Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/user/repo.git
#  * [new tag]         v1.0.1 -> v1.0.1
```

The tag is now fully restored on both your local repository and the remote server. Your teammates can now fetch this tag.

