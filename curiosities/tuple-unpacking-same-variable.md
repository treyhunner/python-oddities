When using a variable name as an index within tuple unpacking, what happens if we assign to the same variable in the same tuple unpacking expression?


## Tuple unpacking 1

Here's a list and a number:

```pycon
>>> numbers = [2, 1, 3, 4, 7, 11, 18, 29]
>>> n = 4
```

Here we're using tuple unpacking to unpack the list into a variable and a list index:

```pycon
>>> numbers[n], n = 0, 2
```

What is the value of `n`?

What is the value of `numbers`?

The `n` variable was reassigned to `2` (the first item in the list):

```pycon
>>> n
2
```

When we assigned to `numbers[n]`, did we assign to `numbers[4]` or `numbers[2]`?

```pycon
>>> numbers
[2, 1, 3, 4, 0, 11, 18, 29]
```

We assigned to `numbers[4]`.


## Tuple unpacking 2

Here we're using tuple unpacking to unpack the list into a variable and a list index:

```pycon
>>> n, numbers[n] = 2, 0
```

What is the value of `n`?

What is the value of `numbers`?

The `n` variable was reassigned to `2` (the first item in the list):

```pycon
>>> n
2
```

When we assigned to `numbers[n]`, did we assign to `numbers[4]` or `numbers[2]`?

```pycon
>>> numbers
[2, 1, 0, 4, 7, 11, 18, 29]
```

We assigned to `numbers[2]`.


## The takeaway

Tuple unpacking assigns item-by-item.

When using tuple unpacking, avoid assigning to a variable that you also use as an index while assigning.
It makes for confusing code.
