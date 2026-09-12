# Strings
> in this module we're learning how python handles text, which is called a string, and how to slice, search and manipulate it.

## Exercise 6.1

## Theory:
- a string is basically just text wrapped in quotes, and we can grab individual characters from it using index numbers
- indexing starts from 0 not 1 which is a bit weird at first
- here i need to find where the colon is in a string and grab everything after it to pull out a number

## Code:
```python
text = "X-DSPAM-Confidence:    0.8475"

colon_pos = text.find(':')

number_str = text[colon_pos + 1:].strip()

number_float = float(number_str)

print(number_float)

```

## Concepts Learnt
- `.find()` returns the index position of a character or substring inside a string
- slicing with `[start:]` grabs everything from that position to the end of the string
- `.strip()` removes any extra whitespace from a string, which is handy after slicing

