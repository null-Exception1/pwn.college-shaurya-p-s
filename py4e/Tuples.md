# Tuples
> in this module we're learning about tuples which are like lists but they cant be changed after they're created, and how they're useful for sorting dictionaries.


## Exercise 10.2

## Theory:
- we can convert a dictionary into a list of tuples using `.items()` and then sort it
- in this instance i need to count how many emails were sent during each hour of the day, then print them out in order by hour

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
    time = words[5]
    
    time_parts = time.split(':')
    hour = time_parts[0]
    
    counts[hour] = counts.get(hour, 0) + 1

sorted_hours = sorted(counts.items())

for hour, count in sorted_hours:
    print(hour, count)

```

## Concepts Learnt
- `sorted()` on a dictionary's `.items()` gives back a list of tuples, sorted by the first item in each tuple by default
- since the tuples here are (hour, count), sorting like this naturally puts them in order by hour
- tuples are useful for sorting because python compares them item by item, starting with the first one
