## 1 Example

## 2 Relations to other algorithmic problems

## 3 Efficient algorithms

![](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1d/LISDemo.gif/800px-LISDemo.gif)

```python
#        S: 0, 8, 4, 12, 2, 10, 6, 14, 1, 9, 5, 13, 3, 11, 7, 15
# BEST_SEQ: 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16  (length)
#           0  8
#           0  4
#           0  4 12
#           0  2 12
#           0  2 10
#           0  2  6
#           0  2  6 14
#           0  1  6 14
#           0  1  6  9
#           0  1  5  9 13
#           0  1  3  9 13
#           0  1  3  9 11
#           0  1  3  7 11 15

from bisect import bisect_left

BEST_SEQ = []

for n in A:
    j = bisect_left(BEST_SEQ, n)

    if j == len(BEST_SEQ):
        BEST_SEQ.append(n)
    else:
        BEST_SEQ[j] = n
```

O(*n* log *n*)

## 4 Length bounds

## 5 Online algorithms

## 6 Application

## 7 See also

- [Patience sorting](../Sorting_algorithm/Patience_sorting.md)
