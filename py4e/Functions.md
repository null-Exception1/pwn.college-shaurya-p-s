# Functions
> in this module we're learning how to bundle up code into reusable blocks called functions so we dont have to keep repeating ourselves.

## Exercise 4.3

## Theory:
- functions can have more than one parameter and we can also give parameters default values
- in this instance i need to build a function that computes pay but with overtime rules built in

## Code:
```python
def computepay(hrs, rate) :
    if hrs > 40 :
        pay = 40 * rate + (hrs - 40) * rate * 1.5
    else :
        pay = hrs * rate
    return pay

hrs = float(input("Enter Hours: "))
rate = float(input("Enter Rate: "))
p = computepay(hrs, rate)
print("Pay:", p)
```

## Concepts Learnt
- functions can take multiple parameters separated by commas
- you can put if/else logic inside a function just like normal code
- breaking code into functions makes it way easier to read and reuse instead of writing it all in one block
