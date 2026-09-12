# Dictionaries
> in this module we're learning about dictionaries which store data as key and value pairs instead of just a plain list of values.


## Exercise 9.4

## Theory:
- we can loop through a dictionary using `.items()` to get both the key and value at the same time
- here i need to find the word that appears the most number of times

## Code:
```python
name = input("Enter file:")
if len(name) < 1:
    name = "mbox-short.txt"
handle = open(name)

counts = dict()

for line in handle:
    if not line.startswith('From '):
        continue

    words = line.split()

    email = words[1]

    counts[email] = counts.get(email, 0) + 1

bigcount = None
bigword = None

for word, count in counts.items():
    if bigcount is None or count > bigcount:
        bigword = word
        bigcount = count

print(bigword, bigcount)

```

## Concepts Learnt
- `.items()` lets us loop through both the key and value together using two variables
- this pattern of "find the biggest so far" using None as a starting point comes up a lot, same as the loops module
- dictionaries plus loops are a really powerful combo for counting and finding patterns in data
