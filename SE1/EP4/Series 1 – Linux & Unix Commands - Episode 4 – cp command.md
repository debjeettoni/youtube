# Series 1 – Linux & Unix Commands | Episode 4 – cp command

## Manual - cp command
```bash
Usage: cp [OPTION]... [-T] SOURCE DEST
  or:  cp [OPTION]... SOURCE... DIRECTORY
  or:  cp [OPTION]... -t DIRECTORY SOURCE...
Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

  -b                           like --backup but does not accept an argument
  -f, --force                  force overwrite files.
  -i, --interactive            prompt before overwrite.
  -l, --link                   hard link files instead of copying
  -L, --dereference            always follow symbolic links in SOURCE
  -n, --no-clobber             do not overwrite an existing file
  -P, --no-dereference         never follow symbolic links in SOURCE
  -p                           preserve the specified attributes
  -R, -r, --recursive          copy directories recursively
  -s, --symbolic-link          make symbolic links instead of copying
  -u, --update                 copy only when the SOURCE file is newer
  -v, --verbose                explain what is being done
```
## Copy a single file
```bash
# Copy file within same directory
cp old.txt new.txt

# Copy file to another directory
cp old.txt one/old.txt

# Copy file from one directory to another
cp one/old.txt two/new.txt
```
## Copy multiple files
```bash
# Copy all files
cp source/* one/

# Copy all files with specific extension
cp source/*.txt two/

# Copy all files with specific name
cp source/data* three/

# Copy all files by name
cp source/India.txt source/China.txt source/Brazil.txt target/
```
## Recursively Copy directories
```bash
# Recursively copy a specific directory and its content's
cp -r source/one/two target1/

# Recursively copy a directory and it's content
cp -r source/* target3/
```
## Backup destination files before copy
```
################### Backup options ###################
# simple or never => Always make simple backups.     #
# numbered or t   => Numbered backups are made.      #
# existing or nil => Numbered if existing backups    #
#                    are numbered, simple otherwise. #
# none or off     => No backups are made (default).  #
######################################################
```
```bash
# Backup destination files before copy
cp source/data.txt target/data.txt
cp -b source/data.txt target/data.txt
cp --backup=simple source/data.txt target/data.txt
cp --backup=numbered source/data.txt target/data.txt
cp --backup=existing source/data.txt target/data.txt
cp --backup=none source/data.txt target/data.txt
```
## Force overwrite destination file
```bash
cp -f source/data.txt target/data.txt 
```
## Prompt before overwriting destination file
```bash
cp -i source/data.txt target/data.txt 
```
## Copy in verbose mode
```bash
cp -r -v source/* target/
```
## Preserve file/directory attributes
```
##############################################
# cp --preserve[=ATTR_LIST] SOURCE TARGET    #
##############################################
# mode: File permissions.                    #
# ownership: User and group ownership.       #
# timestamps: Access and modification times. #
# links: Preserve hard links.                #
# context: Security context.                 #
# xattr: Extended attributes.                #
# all: All of the above attributes.          #
##############################################
```
```bash
# Preserve timestamp
cp --preserve=timestamp source/script.sh target/script.sh

# Preserve ownership
cp --preserve=ownership source/script.sh target/script.sh

# Preserve permissions
cp --preserve=mode source/script.sh target/script.sh

# Preserve all attributes
cp --preserve=all source/script.sh target/script.sh
```
## Copying symbolic links
```bash
# Copy symbolic link target  
cp source/* target2/

# Copy symbolic link itself
cp -P source/* target2/
```
## Prevent accidental overwrites
```bash
# Do not overwrite destination file
cp -n source/file1 target1/file1
```
## Copy only updated files
```bash
# Do not overwrite destination file
cp -u source/file1 target1/file1
```