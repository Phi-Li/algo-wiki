__Source code:__ [`Lib/bisect.py`](https://github.com/python/cpython/blob/master/Lib/bisect.py)

`bisect.`__`bisect_left`__(_`A`_, _`x`_)<br>
`bisect.`__`bisect_left`__(_`A`_, _`x`_, _`lo=0`_, _`hi=len(A)`_)

`bisect.`__`bisect_right`__(_`A`_, _`x`_)<br>
`bisect.`__`bisect_right`__(_`A`_, _`x`_, _`lo=0`_, _`hi=len(A)`_)

<table>
    <thead>
        <tr>
            <th><code>bisect_left(A, x)</code></th>
            <th><code>bisect_right(A, x)</code></th>
        </tr>
    </thead>
    <tbody>
        <tr><td colspan="2" align="center"><code>0 <= i <= len(A)</code></td></tr>
        <tr>
<td>

```
A[lo:i]               A[i:hi]
[ ... (val < x) ... ] [(x) ... (val >= x) ... ]
```

</td>
<td>

```
A[lo:i]                   A[i:hi]
[ ... (val <= x) ... (x)] [ ... (val > x) ... ]
```

</td>
        </tr>
        <tr>
<td>

```python
def bisect_left(A, x, lo=0, hi=None):
    if lo < 0:
        raise ValueError('lo must be non-negative')
    if hi is None:
        hi = len(A)

    while lo < hi:
        mid = (lo + hi) // 2
        # Use __lt__ to match the logic
        #   in list.sort() and in heapq
        if A[mid] < x:
            #  lo              mid
            # [ ... (val < x) ... ] [ ... x
            #                        lo
            lo = mid + 1
        else:
            #       mid               hi
            # ... ][ ... (val >= x) ... ]
            #       hi
            hi = mid

    return lo
```

</td>
<td>

```python
def bisect_right(A, x, lo=0, hi=None):
    if lo < 0:
        raise ValueError('lo must be non-negative')
    if hi is None:
        hi = len(A)

    while lo < hi:
        mid = (lo + hi) // 2
        # Use __lt__ to match the logic
        #   in list.sort() and in heapq
        if x < A[mid]:
            #            mid              hi
            #   x ... ] [ ... (val > x) ... ]
            #            hi
            hi = mid
        else:
            #  lo               mid
            # [ ... (val <= x) ... ] [ ...
            #                         lo
            lo = mid + 1

    return lo
```

</td>
        </tr>
    </tbody>
</table>

`bisect.`__`insort_left`__(_`a`_, _`x`_, _`lo=0`_, _`hi=len(a)`_)<br>
`bisect.`__`insort_right`__(_`a`_, _`x`_, _`lo=0`_, _`hi=len(a)`_)

O(log n) search, slow O(n) insertion.

`bisect.`__`bisect`__(_`a`_, _`x`_, _`lo=0`_, _`hi=len(a)`_)<br>
`bisect.`__`insort`__(_`a`_, _`x`_, _`lo=0`_, _`hi=len(a)`_)

```python
# Create aliases
bisect = bisect_right
insort = insort_right
```
