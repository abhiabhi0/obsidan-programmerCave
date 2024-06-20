Generators in Python are functions that allow you to ==generate a sequence of values over time, instead of computing and storing them all at once==. They are defined using the `yield` keyword instead of `return`. 

==When a generator function is called, it returns a generator object, which can then be iterated over to retrieve values one at a time==.

Here's a simple example of a generator function:

```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1

# Using the generator function
for num in countdown(5):
    print(num)
```

In this example, `countdown()` is a generator function that yields values from `n` down to 1. When the `for` loop iterates over the generator object returned by `countdown(5)`, it prints each value as it's yielded by the generator.

Generators are particularly useful when ==dealing with large datasets or infinite sequences, as they allow you to generate values on-the-fly, conserving memory and improving performance compared to storing all values in memory at once==. 

Additionally, generators are often used in combination with other Python features like list comprehensions and functional programming tools like `map()` and `filter()`.