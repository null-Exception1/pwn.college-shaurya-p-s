# Lists
> in this module we're learning about lists which are used to store multiple values together in one variable, and how to add, remove and loop through them.

## Exercise 8.4

## Theory:
- a list is a collection of values written inside square brackets separated by commas
- lists can hold different types of values and we can access them using index numbers just like strings
- here i need to go through a file and build a list of every unique word in it, then print them out sorted

## Code:
```python
fname = input("Enter file name: ")
fh = open(fname)
lst = list()

for line in fh:
    words = line.split()

    for word in words:
        if word not in lst:
            lst.append(word)

lst.sort()

print(lst)
```

## Concepts Learnt
- `len()` gives the number of items in a list
- we can loop directly over a list to get each item one at a time
- lists are mutable which means we can change them after they're created, unlike strings


## Exercise 8.5

## Theory:
- we can add items to a list using `.append()` and remove using `.remove()` or `del`
- in this instance i need to go through a mailbox file and pull out the email address from every line that starts with 'From ', then count how many there are

## Code:
```python
fname = input("Enter file name: ")
if len(fname) < 1:
    fname = "mbox-short.txt"

fh = open(fname)
count = 0

for line in fh:
    if not line.startswith('From '):
        continue

    words = line.split()

    email = words[1]
    print(email)

    count = count + 1

print("There were", count, "lines in the file with From as the first word")
```

## Concepts Learnt
- `list()` creates an empty list which we can then build up over time
- `.append()` adds a new item to the end of the list
- `max()` and `min()` are built in functions that work directly on lists to give the biggest and smallest values

