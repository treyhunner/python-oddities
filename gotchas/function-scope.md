We have a variable, `n`, which is set to `4`:

```pycon
>>> n = 4
```

Here is a list comprehension that squares numbers:

```pycon
>>> squares = [n**2 for n in range(2, 10)]
>>> squares
[4, 9, 16, 25, 36, 49, 64, 81]
```

That comprehension used the variable `n`.
Did that change the `n` variable that we assigned before the comprehension?

What is the value of `n` now?

```pycon
>>> n
```

The The `n` variable is *still* `4`:

```pycon
>>> n
4
```

Here's a `for` loop that prints out each of the numbers in the `squares` list:

```pycon
>>> for n in squares:
...     print(n)
...
4
9
16
25
49
64
81
```

That loop used the variable `n`.
Did that change the `n` variable that we assigned before the loop?

What is the value of `n` now?

```pycon
>>> n
```

The `n` variable is now `81`:

```pycon
>>> n
81
```

Huh?

What's going on here?
Why did the comprehension *not* update the value of `n` but the `for` loop did?!

This is about how Python's scoping works.

## Scope level

Some languages, like JavaScript, are block-level scoped.
That means that variables are associated with blocks, not functions.
So a variable declared within a block will only affect that block:

```js
> let n = 4;  // assigning outer n to 4
> if (true) {
... let n = 3;  // declaring a *new* n
... }
undefined
> n  // n is still 4
4
```

Python is *not* block-level scoped.

```python
>>> n = 4
>>> if True:
...     n = 3
...
>>> n
3
```

Python is function-level scoped.
So variables are "scoped" to the function they are assigned in, or to the whole module (in the case of global variables).

Our `n` variable is a global variable.
The `n` variable in our `for` loop is also a global variable.

But... what about our comprehension?

Well, comprehensions have their own scope.

```pycon
>>> n = 4
>>> squares = [n**2 for n in range(2, 10)]
>>> n
4
```

It's pretty uncommon to *want* a comprehension's scope to *leak* into the surrounding scope.
If you *do* want that behavior, you could use the [walrus operator][]... though it'll look pretty strange:

```pycon
>>> n = 4
>>> squares = [(n:=x)**2 for x in range(2, 10)]
>>> n
9
```

I really don't recommend writing code like that.
If you need to assign to a variable within an outer scope of your comprehension, use a `for` loop instead.


## Why does it work this way?

You might be thinking "why did the Python core developers make this odd decision to go with function-level scope"?

Well, imagine the alternative.

In Python, variable declarations don't exist.
We see this as a convenience.

So there's no need to distinguish between the *first* use of a variable and the second use.
Assigning to a variable brings that variable into existence.

Since we don't have variable declarations, no syntax exists in the language to distinguish between "should this assign to the inner block or an outer block".
All assignments in Python assign to the inner-most scope.
This is pretty much always what's actually desired... with the current function-level scope that is.

Imagine if there was no easy way to assign a variable to two different values using an `if` statement:

```python
if condition:
    name = "one value"
else:
    name = "a different value"
print(name)  # would this show an error?!
```

We would need an additional way to signal which block variables should be assigned to.

Instead of dealing with any of that mess, Python uses far fewer scopes by not using block-level scoping at all.
