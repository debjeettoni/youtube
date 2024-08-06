# Series 1 – Linux & Unix Commands | Episode 3 – cat command

## Manual - cat command
```bash
Usage: cat [OPTION]... [FILE]...
Concatenate FILE(s) to standard output.

With no FILE, or when FILE is -, read standard input.

  -A, --show-all           equivalent to -vET
  -b, --number-nonblank    number nonempty output lines, overrides -n
  -e                       equivalent to -vE
  -E, --show-ends          display $ at end of each line
  -n, --number             number all output lines
  -s, --squeeze-blank      suppress repeated empty output lines
  -T, --show-tabs          display TAB characters as ^I
  -u                       (ignored)
  -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
```
## Display a file’s contents
```bash
## Using relative path
cat file.txt

## Using absolute path
cat /home/debjeet/dir/subdir/file.txt
```
## Concatenate multiple file’s contents
```bash
## files are in current directory
cat file1.txt file2.txt file3.txt

## files are not in current directory
cat dir1/file1.txt dir2/file2.txt dir3/file3.txt
```
## Save output to a file
```bash
cat file1.txt file2.txt file3.txt > file.txt
```
## Redirection Operators
- >   for output to a file or command
- >>  for appending output to a file or command
- <   for input from a file or command
- <<  for multi-line input from a file or command
## Special use cases in cd command
```
. => current directory
..=> parent directory
~ => home directory
- => previous directory
```
## Create a file with contents using cat and redirection operators
```bash
## Create new content
cat << EOF > file.txt
Hello world!
Welcome to my channel.
EOF

## Append to existing content
cat << EOF >> file.txt
Hello again!
Welcome back.
EOF

## Use variable and command substitution
cat << EOF >> file.txt
This is $USER!
Today is $(date)
EOF
```
## Display line numbers in the output
```bash
## Display line numbers
cat –n countries.txt

## Display line number, excluding blank lines
cat –b countries.txt
```
## Remove blank lines from the output
```bash
## Suppress repeated empty lines from output
cat –ns countries.txt

## Suppress all empty lines from output
cat countries.txt | grep -v '^$'
```
## Display End-of-Line, Tab, and Non-Printable characters
```bash
## Display End-of-Line characters
cat –E special.txt

## Display tab characters
cat –T special.txt

## Display Non-Printable characters
cat –v special.txt

## Display all special characters
cat –A special.txt
```

