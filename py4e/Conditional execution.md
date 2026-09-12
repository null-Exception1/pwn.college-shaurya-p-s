# Conditional execution
> in this module we're learning how to make python take decisions using if, elif and else so the program can behave differently depending on the input.

## Exercise 3.1

## Theory:
- computers can compare two things using comparison operators like `>` `<` `==` `!=`
- if the condition is true then the block under if runs, otherwise it skips

## Code:
```python
hrs = input("Enter Hours:")
rate = input("Enter Rate:")

h = float(hrs)
r = float(rate)

if h <= 40:
    pay = h * r
else:
    regular_pay = 40 * r
    overtime_hours = h - 40
    overtime_pay = overtime_hours * (r * 1.5)
    pay = regular_pay + overtime_pay

print(pay)
```

## Concepts Learnt
- `if` and `else` need a colon at the end
- indentation matters a lot in python, if you dont indent properly it throws an error
- `<=` means less than or equal to


## Exercise 3.3

## Theory:
- sometimes we have more than 2 outcomes so we use `elif` in between if and else
- in this instance i need to convert a score into a letter grade

## Code:
```python
score = input("Enter Score: ")

try:
    s = float(score)
except:
    print("Error: Please enter a numerical value.")
    quit()

if s < 0.0 or s > 1.0:
    print("Error: Score is out of range.")
elif s >= 0.9:
    print("A")
elif s >= 0.8:
    print("B")
elif s >= 0.7:
    print("C")
elif s >= 0.6:
    print("D")
else:
    print("F")

```
