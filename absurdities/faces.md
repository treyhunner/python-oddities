What's this do?

```
>>> O_o = 0_0
```

And this?

```
>>> o_o = 0o0
```

What about this?

```
>>> o_O = 0,0
```

This one?

```pycon
>>> O,O = o_O
```

Take a look:

```pycon
>>> O_o, o_o, O,O, *o_O
(0, 0, 0, 0, 0, 0)
```


## `O_o` assignment explained

Python allows underscores in numeric literals:

```pycon
>>> 1_000_000
1000000
```

But it actually doesn't care about where they're placed

```pycon
>>> 1_0
10
```

Which can be helpful for breaking up binary flags into 4-digit parts:

```pycon
>>> 0b0100_1011
75
```

Any number of `0` digits will make `0` in Python, so `0_0` is `0`:

```
>>> 0_0
0
```

Underscores are allowed in variable names, so `O_o` is simply a variable.

```
>>> O_o = 0_0
```


## o_o assignment explained

The `0o` prefix is used for defining an integer using octal notation (for base-8 numbers).

```pycon
>>> 0o10
8
>>> 0o27
23
```

So `0o0` is `0` and `o_o` is a variable:

```pycon
>>> o_o = 0o0
```


## o_O assignment explained

Putting a comma between two values makes a tuple:

```pycon
>>> 1, 2
(1, 2)
```

There's no whitespace needed, so this assigns the variable `o_O` to a tuple of two `0`s:

```pycon
>>> o_O = 0,0
```


## `O,O` assignment explained

[Tuple unpacking][] uses a `,` on the left-hand side of an assignment:

```pycon
>>> a, b = 1, 2
>>> a
1
>>> b
2
```

And it's possible to assign to the same variable twice with tuple unpacking, though it's quite an odd thing to do:

```pycon
>>> o,o = 0,0
>>> o
0
```

So this unpacks our 2-item `(0, 0)` tuple into the two variables `O` and `O` (which are both the same variable):

```pycon
>>> O,O = o_O
```

Meaning `O` will now point to `0` (the second `0` specifically, not that this matters).


## The reveal explained

Sticking commas between values creates a tuple.

```pycon
>>> 1, 2
(1, 2)
```

A `*` operator can be used to unpack an iterable into another iterable:

```pycon
>>> numbers = [2, 1, 3]
>>> [0, *numbers]
[0, 2, 1, 3]
```

So we're unpacking our two-item `(0, 0)` tuple into the new tuple:

```pycon
>>> O_o, o_o, O,O, *o_O
(0, 0, 0, 0, 0, 0)
```

All the values happened to be `0` with the exception of that last `(0, 0)` tuple that we unpacked.
