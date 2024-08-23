# Series 1 – Linux & Unix Commands | Episode 5 – echo command

## Manual - echo command
```bash
NAME
    echo - Write arguments to the standard output.

SYNOPSIS
    echo [-neE] [arg ...]

DESCRIPTION
    Write arguments to the standard output.
    
    Display the ARGs, separated by a single space character and followed by a
    newline, on the standard output.
    
    Options:
      -n	do not append a newline
      -e	enable interpretation of the following backslash escapes
      -E	explicitly suppress interpretation of backslash escapes
    
    `echo' interprets the following backslash-escaped characters:
      \a	alert (bell)
      \b	backspace
      \c	suppress further output
      \e	escape character
      \E	escape character
      \f	form feed
      \n	new line
      \r	carriage return
      \t	horizontal tab
      \v	vertical tab
      \\	backslash
      \0nnn	the character whose ASCII code is NNN (octal).  NNN can be
    		0 to 3 octal digits
      \xHH	the eight-bit character whose value is HH (hexadecimal).  HH
    		can be one or two hex digits
      \uHHHH	the Unicode character whose value is the hexadecimal value HHHH.
    		HHHH can be one to four hex digits.
      \UHHHHHHHH the Unicode character whose value is the hexadecimal value
    		HHHHHHHH. HHHHHHHH can be one to eight hex digits.
```
## Display a single line of text
```bash
# Display a line of text
echo Hello World
```
## Display special characters
```bash
# Display a line of text containing special characters
echo "Hello World*"
```
## Display a variable value
```bash
# Display a variable value
NAME="Debjeet"
echo "Hello $NAME!"
echo 'Hello $NAME!'
```
## Display another command output
```bash
# Display another command output
echo "Total Memory: $(free -h | grep Mem | awk '{print $2}')"
```
## Display multiple lines
```bash
# Display multiple lines
echo "Hello World.
This is Debjeet.
Welcome!"

#or

echo -e "Hello World.\nThis is Debjeet.\nWelcome!"
```
## Enable and disable interpretation of backslash escape characters
```bash
# Enable
echo -e "Hello\nNew Line\n\tTab\vVertical Tab\n\\ Backslash"

# Disable
echo -E "Hello\nNew Line\n\tTab\vVertical Tab\n\\ Backslash"
```
## Display single and double quotes
```bash
# Display double quotes
echo "\"Hello World\""

# Display single quotes
echo "'Hello World'"

# Display single quotes within double quotes
echo "\"Hello 'World'\""

# Display single quotes within single quotes
echo "'Hello 'World''"

# Display double quotes within single quotes
echo "'Hello \"World\"'"

# Display double quotes within double quotes
echo "\"Hello \"World\"\""

# Display variable value in quotes
NAME="Debjeet"
echo "Hello \"$NAME\""
echo "Hello '$NAME'"
```
## Write to a file
```bash
# Overwrite
echo "Hello World" > data.txt

# Append
echo "This is Debjeet" >> data.txt
```
