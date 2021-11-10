---
title: "Design"
weight: 1
draft: false
authors: ["Antonio Kam"]
---

# Design

## Abstraction

Abstraction (in a CS context) is broadly understood as extracting relevant information from other sources in order to simplify the codebase.

Abstraction is very useful if you're working on a project with multiple people involved. By giving people a means of accessing information in a more abstract way, they do not need to understand the code that you've written, but instead, only need to know what said abstractions do.

### Abstraction by Parameterization

This is pretty much **DRY** (Don't Repeat Yourself). Instead of hard-coding code that have similar structure, you could write a function that takes in parameters (arguments) which can be used to do the same thing. (Taken from CS61A Fall 2021 Design Slides)

```python
interest =  1 + 0.6 * 2
interest2 =  1 + 0.9 * 4
interest3 =  1 + 2.1 * 3
```

As you can see above, there is a bunch of redundant code, which could be condensed into the function below (and then subsequently called with its respective arguments)

```python
def interest(rate, years):
    return 1 + rate * years
```

In this example, the removed detail, or the abstracted detail are **the values themselves**.

### Abstraction by Specification

Instead of building your own `round` function, you could just use Python's built-in implementation, has its own specification for the inputs that it takes in (a number and the number of digits to round to), and then outputs a number based on that.

As a result, you could use this implementation of the `round` function in your code to save from **implementing the code yourself.**

### Using an Abstraction

Say we have a specification for `square(n)` that takes an input `n` and returns the square of `n`. If we had this implemented already, we could use this function in our code. As a result, the code block below would run without any problems due to this abstraction.

```python
def sum_squares(x, y):
    return square(x) + square(y)
```

### Implementing the Abstraction

There are many valid ways to implement this function, but some pieces of code are less efficient or have certain effects that you may not intend for it to have.

For example, take a look at the possible implementations of `square`:

```python
def square(n):
    return n**2

square = lambda n: n*n

square = lambda n: (n*(n-1)) + n
```

Notice how the bottom implementation takes more computational resources than the 2 implementations above? This will be more important as the pieces of code written get more complicated — the size of the program and the time efficiency can be effected by poor implementations.  

## Reasonable Names

### Choosing Names

While the computer does not care about names, it greatly improves readability for humans — even yourself when you look back at the code at a later date. 

Names should convey the meaning or purpose of the values/functions bound to them. Function names specifically usually have names that convey their effect, their behaviour, or the value returned.

### Parameter Names for Functions

Functions take in parameter names — these too can either convey meaning, or be needlessly confusing for human readers. 

These parameters could also be explained in the docstring, typically placed directly below the function signature.

```python
def summing_function(n, func):
    """Sums the result of applying the function func n times from 1 to N.
    n: int
    func(n: int): function -> int
    summing_function -> int
    """
    total = 0
    for i in range(1, n + 1):
        total += func(i)
    return total
```

### Redundant Code

Names should also be used if a compound expression has been repeated. For example:

```python
if a == b and b > c:
    print(a == b and b > c)
```

You can instead store `a == b and b > c` in a variable, then use that variable for the expressions later. (I understand that the code above is very useless, but it does demonstrate a point)

```python
result = a == b and b > c
if result:
    print(result)
```

### More Naming Tips

Typically, names with one character are not very helpful in real codes: names should be more descriptive than that. However, there are exceptions:

- `n`, `k`, `i`: Usually used to denote an integer
- `x`, `y`, `z`: Usually real numbers/coordinates/number inputs for functions
- `f`, `g`, `h`: Shorthand for function names

Names should be long if they help self-document your code! 

## Debugging + Errors

### Types of Errors

There are different errors in Python, the most common being the following:

- Logic Error
- Syntax Error
- Runtime Error

### Logic Error

A program has a logic error if the program does not behave as expected. This will not necessarily output something in the console, but can sometimes be discovered as a failing test or a bug report from users. This is why it's important to write **doctests** for the outputs you expect — you can be sure that the code does not have logic errors.

One (very common) example of a logic error is the off-by-one error, where you were off by one in your implementation of a while/for loop.

### Syntax Error

Each programming language has its own syntactic rules. If the rules aren't followed, the program will not necessarily be able to parse the code, thus resulting in code that cannot be executed at all. To fix these syntax errors, use the Python error log in the console to see the traceback message and see which lines they occurred at.

### Runtime Errors

A very common example of a runtime error is the ZeroDivisionError. These errors will happen **while** a program is running, and in the case of Python, it stops the execution of the program completely.

### Tracebacks

When reading a Python traceback (which occurs when an error occurs), make sure to look from the bottom to the top. That way, you can see the error that occurred, and the most recent calls before the error happened.