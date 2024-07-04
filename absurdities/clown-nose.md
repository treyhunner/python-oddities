A tuple:

```pycon
>>> ()
()
```

A list:

```pycon
>>> []
[]
```

A dictionary:

```pycon
>>> {}
{}
```

A dictionary wearing a clown nose:

```pycon
>>> {*()}
set()
```

Normally we make an empty set by calling the `set` constructor: `set()`.

Running `{*()}` is actually *slightly* faster because there's no name lookup or function call.

But please don't replace your `set()` calls with `{*()}`... it's too absurd and likely not worth any potential performance benefit.
