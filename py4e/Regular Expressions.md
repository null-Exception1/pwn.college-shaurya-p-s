# Regular Expressions
> in this module we're learning about regular expressions (regex) which is basically a way to search for patterns in text instead of exact matches.

## Exercise 11.1

```py
import re

fname = input("Enter file name: ")
if len(fname) < 1:
    fname = "regex_sum_2466158.txt"

try:
    handle = open(fname)
except FileNotFoundError:
    print(f"File {fname} not found. Make sure it's in the same folder.")
    quit()

total_sum = 0

for line in handle:
    numbers = re.findall('[0-9]+', line)

    for num in numbers:
        total_sum += int(num)

print("Sum:", total_sum)
```

## Concepts Learnt
- `re.findall()` returns a list of all matches instead of just true/false like search does
- `[0-9]+` is a character class that matches one or more digits in a row
- since findall gives back strings, we still have to convert them using `int()` before doing any math on them, cant just add strings directly
