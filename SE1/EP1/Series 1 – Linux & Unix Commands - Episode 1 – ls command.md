# Series 1 – Linux & Unix Commands | Episode 1 – ls command

## List contents of current directory
```bash
ls
```
## List contents of a specific directory
```bash
ls mydir/
```
## List hidden files
```bash
ls -a
```
## List hidden files excluding dot’s
```bash
ls -A
```
## List contents in long format
```bash
ls -l
```
## Combine multiple options
```bash
ls -Al
```
## Get file or directory size in KB, MB, or GB
```bash
ls -Alh
```
## Recursively lists all the contents of a directory
```bash
ls -Rl
```
## Sort output in ls command (default)
```bash
ls -lh
```
## Sort by modified time
```bash
ls -lht
```
## Sort by access time
```bash
ls -lt --time=atime
```
## Sort by change time
```bash
ls -lt --time=ctime
```
## Sort by file extension
```bash
ls -l --sort=extension
```
## Sort by file size
```bash
ls -lhS
```
## Reverse the sorting order
```bash
ls -lrh
```
## List only directories
```bash
ls -ld */
```
## List files with specific extension
```bash
ls -l *.txt
```
