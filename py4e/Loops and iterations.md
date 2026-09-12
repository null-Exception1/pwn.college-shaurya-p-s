# Loops and iterations
> in this module we're learning how to repeat a block of code multiple times using while loops and for loops instead of writing the same line again and again.

## Exercise 5.2

## Theory:
- `continue` skips the rest of the current loop and goes back to the top, its different from break because it doesnt stop the loop completely
- in this instance i need to keep asking for numbers until the user types done, and find the biggest and smallest while skipping any invalid input using continue

## Code:
```python
largest = None
smallest = None

while True:
    num = input("Enter a number: ")
    if num == "done":
        break
    
    try:
        inum = int(num)
    except:
        print("Invalid input")
        continue

    if largest is None or inum > largest:
        largest = inum
        
    if smallest is None or inum < smallest:
        smallest = inum

print("Maximum is", largest)
print("Minimum is", smallest)

```

## Concepts Learnt
- `continue` jumps back to the start of the loop and skips whatever code comes after it in that pass
- using `None` as a starting value for largest/smallest means we dont need to guess a starting number before any input comes in
- break stops the whole loop, continue only skips the current round
