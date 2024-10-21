What's going on in this code?

```python
for (n) in (range) (3) : (print) (n)
```

This is valid code that prints out the numbers 0, 1, and 2:

```pycon
>>> for (n) in (range) (3) : (print) (n)
...
0
1
2
```

But *why* is it valid?


## The parentheses around function names

When calling a function any whitespace between the function name and the parentheses used to call the function is ignored:

```pycon
>>> range (3)
range(0, 3)
>>> print     (2)
2
```

But That explains the space in `(range) (3)` and `(print) (n)`.
But what about the first set of parentheses?

Well, `range` and `print` are variables that point to function objects:

```pycon
>>> range
<class 'range'>
>>> print
<built-in function print>
```

That's how functions work in Python.
The function name is a variable and it points to a *[callable][]* object.

**Note**: technically `range` is a class, as are many built-in "functions" in Python, but it's usually referred to as a function (see [Python's functions are sometimes classes][callable article]).

So functions are callable objects and `range` and `print` are variables that point to those callable objects.

What do parentheses around a variable do?

```pycon
>>> x = 4
>>> (x)
4
```

Nothing.

Those parentheses are used for grouping, the same way these parentheses are:

```pycon
>>> (2 + 3) * 7
35
```

We just happen to be grouping just one value, which doesn't do anything in particular.

So these two expressions are valid because the parentheses around the function name are just parentheses around a variable that don't do anything in particular:

```pycon
>>> range
<class 'range'>
>>> print
<built-in function print>
```


## Parentheses around `n`

What about the first set of parentheses here?

```pycon
>>> for (n) in (range) (3) : (print) (n)
...
0
1
2
```

Well, for some reason parentheses are valid in an assignment like this:

```pycon
(n) = 3
```

My best guess as to "why" is tuple unpacking.

But wait... there's no tuple being unpacked.

Unpacking a single-item iterable would look like this:

```pycon
>>> iterable = [3]
>>> (n,) = iterable
```

Or maybe like this:

```pycon
>>> [n] = iterable
```

But we aren't actually unpacking an iterable as we loop over `range`.
Instead we're assigning to a number.

I'm guessing that the allowance of parentheses around a value in an assignment is somehow related to the parentheses used for tuple unpacking.
This syntax does not appear to have an actual purpose and I'm guessing it is supported due to an accident of history.


## One line of code?

Why is this all one line of code?

```python
for (n) in (range) (3) : (print) (n)
```

What makes that valid?

Statements that involve a block of code start with a line that ends in a colon to start the code block:

```python
for (n) in (range) (3):
    (print) (n)
```

When the block of code is just a single line, it's valid to put the whole block in a single line:

```python
for (n) in (range) (3): (print) (n)
```

This is true of `if`, `for`, `def`, `with`, `class`, `while`, `else`, and any other block... though you can't nest blocks.
So this is *not* valid:

```pycon
>>> for n in range(3): if n == 0: print(n)
  File "<python-input-23>", line 1
    for n in range(3): if n == 0: print(n)
                       ^^
SyntaxError: invalid syntax
```

The only remaining thing be explained in that line of code is the space before the `:`.

That is another of the *many* places that Python allows spaces to be.

So any number of spaces are valid before or after that colon:

```pycon
>>> for n in range(3)     :  print(n)
...
0
1
2
```

In fact, Python ignores additional whitespace just about anywhere except at the beginning of a statement (that's significant indentation).


## You can but you shouldn't

Just in case it's not clear, this code is confusing:

```python
for (n) in (range) (3) : (print) (n)
```

I, and most Python developers, would prefer to see that code formatted like this instead:

```python
for n in range(3):
    print(n)
```


[callable]: https://www.pythonmorsels.com/callables/
[callable article]: https://www.pythonmorsels.com/class-function-and-callable/
