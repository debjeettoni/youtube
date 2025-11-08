## How to configure git (`git config` command)?

### **Agenda**

* What is Git Configuration?
* The Three Configuration Levels (System, Global, Local)
* Basic Configuration: Setting Your Identity
* Viewing Your Configuration Settings
* Configuring Your Default Editor
* Advanced Configuration: Creating Aliases
* Summary: Key Takeaways

---

## What is Git Configuration?

The `git config` command is your control panel for Git. It lets you customize how Git looks and behaves on your machine.

### **Why is it important?**

* **Identity**: It tells Git **who you are**. Every commit you make is stamped with your name and email.
* **Customization**: You can change settings to match your personal preferences, like the default text editor for writing commit messages.
* **Efficiency**: You can create **aliases** (shortcuts) for long commands to speed up your workflow.
* **Behavior**: You can change default behaviors, like how Git handles line endings or displays color output in the terminal.

Essentially, `git config` makes Git work for **you**.

---

## The Three Configuration Levels

Git stores its configuration settings in three different places, or "levels." This allows you to apply settings broadly or just for a specific project.

### **System**
* **Scope**: Applies to **every user** on the entire machine.
* **File Location**: `(prefix)/etc/gitconfig`
* This is the highest level, but the least common to modify.

### **Global**
* **Scope**: Applies to **you**, the current user, across all of your repositories.
* **File Location**: `~/.gitconfig` or `~/.config/git/config`
* This is where you'll set your personal details like name and email.

### **Local**
* **Scope**: Applies **only** to the specific repository you are currently in.
* **File Location**: `.git/config` inside your project directory.
* This level overrides any conflicting settings from the global and system levels.

---

## Basic Configuration: Setting Your Identity

This is the very first thing you should do after installing Git. Every commit you make is tagged with this information, making it essential for tracking contributions.

### **Setting Your Username**

Use the `--global` flag to set your name for all your repositories. Replace "Your Name Here" with your actual name.

```bash
git config --global user.name "Your Name Here"
```

### **Setting Your Email**

Similarly, set the email that will be associated with your commits. Use the email you've linked to your GitHub or GitLab account.

```bash
git config --global user.email "youremail@example.com"
```

---

## Viewing Your Configuration: The Big Picture

The most direct way to see your settings is to list everything Git knows about.

### **Listing Combined Settings**

To see a complete list of all your Git settings from all levels (system, global, and local), use the `--list` flag. Git shows the final, active value for each key.

```bash
git config --list
```

**Example Output:**

```bash
user.name=Your Name Here
user.email=youremail@example.com
core.editor=code --wait
...
```

This command is perfect for getting a quick overview of your current Git environment.

---

## Viewing Your Configuration: Getting Specific

You can also narrow your search to see settings from a specific level or to check a single value.

### **Listing Settings by Level**

This is useful for debugging to see exactly where a setting is coming from.

```bash
# View global-level settings only
git config --list --global

# View local (repository-specific) settings only
git config --list --local
```

### **Getting a Specific Value**

To see the value for a single key, provide the key's name.

```bash
git config user.name
```

**Example Output:**

```bash
Your Name Here
```

---

## Configuring Your Default Editor

When you perform actions like creating a commit without the `-m` flag, Git opens a text editor for you to write a detailed message. You can change the default editor (often Vim) to one you prefer.

### **How to Set Your Editor**

You can set your favorite editor using the `core.editor` setting.

**Example for Visual Studio Code:**
The `--wait` flag is crucial; it makes Git wait until you save and close the file in VS Code.

```bash
git config --global core.editor "code --wait"
```

**Example for Nano:**

```bash
git config --global core.editor "nano"
```

**Example for Vim (or to set it back):**

```bash
git config --global core.editor "vim"
```

---

## Advanced Configuration: Creating Aliases

Aliases are custom shortcuts that you can create for longer or frequently used Git commands. They are a fantastic way to save time and reduce typing.

### **What is an Alias?**

An alias is a way to tell Git, "When I type this short command, I actually mean to run this longer command."

### **How to Create an Alias**

You create an alias by configuring a key that starts with `alias.` followed by your shortcut name.

**Example: An alias for `git status`**

```bash
git config --global alias.st status
```

Now, running `git st` will have the exact same effect as running `git status`.

---

## Advanced Configuration: Useful Alias Examples

You can create aliases for almost any command, from simple shortcuts to more complex operations.

### **Simple Command Aliases**

Here are a few common aliases to save keystrokes on daily commands.

```bash
# co for checkout
git config --global alias.co checkout

# br for branch
git config --global alias.br branch

# ci for commit
git config --global alias.ci commit
```

### **Complex Command Alias**

You can also create an alias for a command that includes flags and arguments.

```bash
# An alias to see the last commit
git config --global alias.last "log -1 HEAD"
```

Now, `git last` will show you the most recent commit's details.

---

## Summary: Key Takeaways

Let's quickly review the key points for configuring Git.

* **`git config` is Your Control Panel**: It's the command you use to customize your Git settings.
* **Three Levels of Configuration**: Git uses **local**, **global**, and **system** levels, with local settings having the highest priority.
* **Set Your Identity First**: Always configure your `user.name` and `user.email` globally. This is essential for tracking your work.
* **View Settings Easily**: Use `git config --list` to see all your active settings and `git config <key>` to check a specific one.
* **Customize Your Workflow**: You can change your default editor and create powerful aliases to make Git work faster for you.

