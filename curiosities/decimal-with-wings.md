`_0_` isn't an integer:

```pycon
>>> int("_0_")
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    int("_0_")
    ~~~^^^^^^^
ValueError: invalid literal for int() with base 10: '_0_'
```

`_0_` isn't a floating point number:

```pycon
>>> float("_0_")
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    float("_0_")
    ~~~~~^^^^^^^
ValueError: could not convert string to float: '_0_'
```

`_0_` isn't a complex number:

```pycon
>>> complex("_0_")
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    complex("_0_")
    ~~~~~~~^^^^^^^
ValueError: could not convert string to complex: '_0_'
```

`_0_` isn't a fraction:

```pycon
>>> from fractions import Fraction
>>> Fraction("_0_")
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    Fraction("_0_")
    ~~~~~~~~^^^^^^^
  File "/usr/lib/python3.13/fractions.py", line 256, in __new__
    raise ValueError('Invalid literal for Fraction: %r' %
                     numerator)
ValueError: Invalid literal for Fraction: '_0_'
```

But `_0_` *is* a decimal number:

```pycon
>>> from decimal import Decimal
>>> Decimal("_0_")
Decimal('0')
```

## What's the `_`?

Python's numbers can optionally use underscores to visually separate groups of digits.
These are usually used to group numbers in thousands:

```pycon
>>> 1_000_000
1000000
```


## What's going on here?

Most of Python's numeric types disallow leading underscores, trailing underscores, and double underscores:

```pycon
>>> int("1_000__000")
Traceback (most recent call last):
  File "<python-input-15>", line 1, in <module>
    int("1_000__000")
    ~~~^^^^^^^^^^^^^^
ValueError: invalid literal for int() with base 10: '1_000__000'
>>> int("1_000_000_")
Traceback (most recent call last):
  File "<python-input-16>", line 1, in <module>
    int("1_000_000_")
    ~~~^^^^^^^^^^^^^^
ValueError: invalid literal for int() with base 10: '1_000_000_'
>>> int("_1_000_000")
Traceback (most recent call last):
  File "<python-input-17>", line 1, in <module>
    int("_1_000_000")
    ~~~^^^^^^^^^^^^^^
ValueError: invalid literal for int() with base 10: '_1_000_000'
```

But the `decimal.Decimal` type seems to be more lenient in its parsing:

```pycon
>>> Decimal("____0____.____0____")
Decimal('0.0')
```

Thanks to [Glyph](https://mastodon.social/@glyph/112729486996460973), I'll now be calling this "Totoro's Zero".


## CPython bug

I decided to [open an issue for this](https://github.com/python/cpython/issues/121382) since this inconsistency *is* a bug (albeit a minor one).
