# Files
> in this module we're learning how to open, read and work with files instead of just typing everything manually into the program.

## Exercise 7.1

## Theory:
- to work with a file we first need to open it using `open()` which gives us a file handle
- we can loop through the file line by line just like a list
- here i need to open a file and print every line in it

## Code:
```python
fname = input("Enter file name: ")
fhand = open(fname)

for line in fhand :
    print(line)
```

## Concepts Learnt
- `open()` creates a file handle which is like a connection to the file, not the actual content yet
- we can use a for loop directly on the file handle to go through it line by line
- if the file doesnt exist this throws an error called FileNotFoundError


## Exercise 7.2

## Theory:
- every line read from a file comes with an invisible newline character `\n` at the end, so we usually need to strip it
- in this instance i need to count how many lines in the file contain a certain word

## Code:
```python
fname = input("Enter file name: ")
fhand = open(fname)

count = 0
for line in fhand :
    line = line.rstrip()
    if "python" in line :
        count = count + 1

print("Count:", count)
```

## Concepts Learnt
- `.rstrip()` removes whitespace/newline characters from the end of a string
- we can keep a running total using a counter variable that increases inside the loop
- reading files line by line is more memory efficient than loading the whole thing at once


## Exercise 7.3

## Theory:
- we can also use try/except with files to handle the case where the file doesnt actually exist
- here i need to safely try to open a file and show a friendly message if it fails

## Code:
```python
fname = input("Enter file name: ")

try :
    fhand = open(fname)
except :
    print("File cannot be opened:", fname)
    exit()

count = 0
for line in fhand :
    if line.startswith("Subject:") :
        count = count + 1

print("There were", count, "subject lines in", fname)
```

## Concepts Learnt
- wrapping `open()` in a try/except stops the program from crashing when the file is missing
- `exit()` is used to stop the program completely if something critical fails
- `.startswith()` is really useful for filtering specific lines out of a big file
