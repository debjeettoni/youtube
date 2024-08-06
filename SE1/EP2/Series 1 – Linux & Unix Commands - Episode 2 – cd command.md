# Series 1 – Linux & Unix Commands | Episode 2 – cd command

## Manual - cd command
```bash
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.

    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable. If DIR is "-", it is converted to $OLDPWD.

    Options:
      -L        force symbolic links to be followed: resolve symbolic
                links in DIR after processing instances of `..'
      -P        use the physical directory structure without following
                symbolic links: resolve symbolic links in DIR before
                processing instances of `..'
      -e        if the -P option is supplied, and the current working
                directory cannot be determined successfully, exit with
                a non-zero status
      -@        on systems that support it, present a file with extended
                attributes as a directory containing the file attributes
```
## Directory structure
```bash
/
└── home
    └── debjeet
        └── directory
            └── file
```
## Navigate to home directory
```bash
cd
```
### or
```bash
cd ~
```
## Directory structure
```bash
/
└── home
    └── debjeet
        ├── dir1
        │   └── subdir1
        └── dir2
            └── subdir2
```
## Navigate to specific directory
```bash
cd dir1/subdir1/

## or

cd /home/debjeet/dir2/subdir2/
```
## Special use cases in cd command
```
. => current directory
..=> parent directory
~ => home directory
- => previous directory
```
## Special use cases in cd command examples
```bash
## Navigate to current working directory
cd .

## Navigate to parent working directory
cd ..

## Navigate to user's home directory
cd ~

## Navigate to last working directory
cd -
```
## Navigate to directory name having space
```bash
cd My\ Directory

## or

cd "My Directory"

## or 

cd 'My Directory"
```
## Navigate to directory name having special character
```bash
cd \$mydir
```
## Directory Structure
```bash
.
├── dir1
│   └── file1
└── dir2
    └── symlink_to_dir1 -> /home/debjeet/dir1
```
## Navigate symbolic links
```bash
## L - Follow Symbolic Links
cd -L /home/debjeet/dir2/symlink_to_dir1
cd -L ..
# Results in /home/debjeet

## P - Ignore Symbolic Links
cd -P /home/debjeet/dir2/symlink_to_dir1
cd -P ..
# Results in /debjeet/user/dir2
```
