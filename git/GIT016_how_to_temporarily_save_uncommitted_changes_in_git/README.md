## How to Save Your Changes (`git stash`)

`git stash` is a command that temporarily shelves (or "stashes") your uncommitted changes—both staged and unstaged—allowing you to revert to a clean working directory.

This is incredibly useful when you're in the middle of a change but need to quickly switch branches to fix a bug or pull in an urgent update, without having to make an incomplete commit.

### **The Command**

```bash
# Stash all tracked files (staged and unstaged)
git stash

# You can also stash with a specific message
git stash save "Your custom message"
```

### **Example:**

You are working on `index.html` and `styles.css` but need to switch back to `main` immediately.

```bash
# Step 1: Check your status (you have uncommitted changes)
git status
# On branch feature/login
# Changes not staged for commit:
#       modified:   index.html
# Changes to be committed:
#       modified:   styles.css

# Step 2: Stash the changes
git stash
# Saved working directory and index state WIP on feature/login: a1b2c3d Feat: Add login form

# Step 3: Check your status again (it's clean!)
git status
# On branch feature/login
# nothing to commit, working tree clean
```

Your changes are now safely stored in the stash, and your working directory is clean, allowing you to switch branches safely.

---

## How to List Your Stashes (`git stash list`)

After you've stashed some changes, you might lose track of what you've saved. Git provides a simple command to show you a list of all your stashes, which are stored in a "stack."

This command shows you each stash, its automatically generated name (like `stash@{0}`), the branch it was created on, and the message you provided.

### **The Command**

```bash
# Show a simple list of all stashes
git stash list
```

### **Example:**

You have stashed changes twice, once with a custom message.

```bash
# Step 1: Run the list command
git stash list

# Step 2: Review the output
# The output is in "Last-In, First-Out" (LIFO) order:
# stash@{0}: On feature/navbar: WIP: Refactor navbar component
# stash@{1}: WIP on feature/login: a1b2c3d Feat: Add login form
```

  * `stash@{0}` is your **most recent** stash.
  * `stash@{1}` is your oldest stash.

This list helps you identify which stash you want to re-apply.

---

## How to Re-apply Your Changes (`git stash pop`)

This is the most common way to get your stashed changes back. The `git stash pop` command does two things in one step:

1.  **Applies** the changes from the most recent stash (`stash@{0}`) to your working directory.
2.  **Deletes** (or "drops") that stash from your stash list.

Use this command when you are ready to resume your work and don't need the stash anymore.

### **The Command**

```bash
# Apply the most recent stash AND delete it from the list
git stash pop

# You can also "pop" a specific stash by its ID
git stash pop stash@{1}
```

### **Example (Using `pop`):**

You are back on your branch and want to resume your work.

```bash
# Step 1: Check your stash list
git stash list
#   stash@{0}: WIP on feature/login: a1b2c3d Feat: Add login form

# Step 2: Pop the stash
git stash pop
# On branch feature/login
# Changes not staged for commit:
#       modified:   index.html
# Changes to be committed:
#       modified:   styles.css
# Dropped stash@{0} (a1b2c3d...)

# Step 3: Check your stash list again (it's empty)
git stash list
# (Returns nothing)
```

Your files are restored, and the stash has been automatically removed.

---

## How to Re-apply Your Changes (`git stash apply`)

This is a more specialized command. The `git stash apply` command **applies** the changes from a stash, but it **does not** delete the stash from your list.

This is useful if you want to apply the same set of stashed changes to **multiple branches**.

Because `apply` does not delete the stash, you will have to clean it up later using the `git stash drop` command.

### **The Command**

```bash
# Apply the most recent stash and KEEP it in the list
git stash apply

# Apply a specific stash and KEEP it
git stash apply stash@{1}
```

### **Example (Using `apply`):**

You want to apply your changes but keep the stash to use elsewhere.

```bash
# Step 1: Check your stash list
git stash list
#   stash@{0}: WIP on feature/login: a1b2c3d Feat: Add login form

# Step 2: Apply the stash
git stash apply
# On branch feature/login
# Changes not staged for commit:
#       modified:   index.html
# Changes to be committed:
#       modified:   styles.css

# Step 3: Check your stash list again (it's still there)
git stash list
#   stash@{0}: WIP on feature/login: a1b2c3d Feat: Add login form
```

Your files are restored, but the stash remains in your list.

---

## How to Delete a Stash (`git stash drop`)

This is the **safe and recommended** way to clean up your stash list, especially after using `git stash apply`.

The `git stash drop` command deletes **one specific** stash from your list. If you don't provide an ID, it will delete the most recent stash (`stash@{0}`). This allows you to remove old stashes one by one without accidentally deleting work you still need.

### **The Command**

```bash
# Delete the most recent stash (stash@{0})
git stash drop

# Delete a specific stash by its ID
git stash drop stash@{1}
```

### **Example:**

You have two stashes and want to delete the older one (`stash@{1}`).

```bash
# Step 1: Check your stash list
git stash list
#   stash@{0}: WIP on feature/navbar: 5f6g7h8 Add nav links
#   stash@{1}: WIP on feature/login: a1b2c3d Feat: Add login form

# Step 2: Drop the older stash
git stash drop stash@{1}
# Dropped stash@{1} (a1b2c3d...)

# Step 3: Check your stash list again
git stash list
#   stash@{0}: WIP on feature/navbar: 5f6g7h8 Add nav links
```

Your stash list is now clean, and `stash@{0}` is safe.

---

## How to Delete *All* Stashes (`git stash clear`)

This is a **destructive** and **irreversible** command. `git stash clear` will instantly **delete every stash** you have saved, without any confirmation.

You should only use this command when you are 100% certain that you do not need *any* of the changes saved in *any* of your stashes.

### **The Command**

```bash
# WARNING: This permanently deletes ALL stashes.
git stash clear
```

### **Example:**

You have multiple stashes and want to permanently delete all of them.

```bash
# Step 1: Check your stash list (you have two)
git stash list
#   stash@{0}: WIP on feature/navbar: 5f6g7h8 Add nav links
#   stash@{1}: WIP on feature/login: a1b2c3d Feat: Add login form

# Step 2: Run the clear command
git stash clear

# Step 3: Check your stash list again
git stash list
# (Returns nothing)
```

All of your stashes are now permanently gone.