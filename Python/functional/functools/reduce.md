`functools.`__`reduce`__(_`function`_, _`iterable`_[, _`initializer`_])

```python
def reduce(function, sequence, initializer=None):
    it = iter(sequence)

    if initializer is None:
        try:
            value = next(it)
        except StopIteration:
            raise TypeError("reduce() of empty sequence with no initial value") from None
    else:
        value = initializer

    for element in it:
        value = function(value, element)

    return value
```

__Examples__

Implement `sum()`:

```python
from functools import reduce
from operator import add

def sum(iterable):
    return reduce(add, iterable)
```

Implement `max()`/`min()`:

```python
from functools import reduce

def max(iterable):
    return reduce(lambda x, y: x if x > y else y, iterable)

def min(iterable):
    return reduce(lambda x, y: x if x < y else y, iterable)
```

See also `itertools.accumulate()`
