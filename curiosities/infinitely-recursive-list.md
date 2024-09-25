Here's an empty list:

```pycon
>>> x = []
```

What do you think *this* might do?

```pycon
>>> x.append(x)
```

What would it result in?

An empty list?

A single-element list with an empty list itself?

A list-containing-a-list-containing-a-list?

Something else?

It turns out...

```pycon
>>> x[0] is x
True
```

It's a list containing itself.

Because that's possible in Python.
[Data structures don't contain objects in Python](https://www.pythonmorsels.com/data-structures-contain-pointers/), they contain pointers or references to objects.

So a list can contain itself.
But wait... what's the string representation of such an object?

```pycon
>>> x
[[...]]
```

Fortunately, when showing the string representation of a list or a dictionary Python checks for the possibility of the object "containing" itself (rather containing a *reference* to itself) and it uses `...` to represent that.
I say fortunately because otherwise Python would need to print an infinite number of `[`...`]` angle brackets.

This is something the `list` and `dict` classes *specially* handle.

If we made our own class tried to look at a string representation of an object that pointed to itself, by default we'd get a recursion error:

```pycon
>>> class Thing:
...     def __init__(self, value):
...         self.value = value
...     def __repr__(self):
...         return f"Thing({self.value!r})"
...
>>> t = Thing(4)
>>> t
Thing(4)
>>> t.value = t
>>> t
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    t
  File "<python-input-1>", line 5, in __repr__
    return f"Thing({self.value!r})"
                   ^^^^^^^^^^^^^^
  File "<python-input-1>", line 5, in __repr__
    return f"Thing({self.value!r})"
                   ^^^^^^^^^^^^^^
  File "<python-input-1>", line 5, in __repr__
    return f"Thing({self.value!r})"
                   ^^^^^^^^^^^^^^
  [Previous line repeated 988 more times]
RecursionError: maximum recursion depth exceeded
```

We could handle the possibility of a recursive string representation by using [`reprlib.recursive_repr`](https://docs.python.org/3/library/reprlib.html#reprlib.recursive_repr):

```pycon
>>> from reprlib import recursive_repr
>>> class Thing:
...     def __init__(self, value):
...         self.value = value
...     @recursive_repr()
...     def __repr__(self):
...         return f"Thing({self.value!r})"
...
>>> t = Thing(4)
>>> t
Thing(4)
>>> t.value = t
>>> t
Thing(...)
```

I pretty much never use this helper because I rarely encounter the possibility of someone reasonably wanting to have an object that happens to contain a pointer either to itself or to another object that points back to itself.
