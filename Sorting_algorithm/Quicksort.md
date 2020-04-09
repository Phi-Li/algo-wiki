# Quicksort

![Sorting quicksort anim.gif](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

```python
def part(A: list, L: int, R: int):
    if L < R:
        p = exchange(A, L, R)

        part(A, L, p-1)
        part(A, p+1, R)

# [3,7,8,5,2,1,9,5]     [4]
# [3] [7,8,5,2,1] [9,5] [4]
# [3,1] [8,5,2] [7,9,5] [4]
#      R   L
# [3,1,2] [5,8,7,9,5]   [4]
# [3,1,2] [4] [8,7,9,5,5]
def exchange(A: list, L: int, R: int) -> int:
    pivot = A[R]

    i, j = L, R-1
    while i < R and A[i] < pivot:
        i += 1
    while L < j and A[j] >= pivot:
        j -= 1

    while i < j:
        A[i], A[j] = A[j], A[i]

        while i < R and A[i] < pivot:
            i += 1
        while j > L and A[j] >= pivot:
            j -= 1

    A[i], A[R] = A[R], A[i]
    return i
```

## 1 History

## 2 Algorithm

![Quicksort-diagram.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Quicksort-diagram.svg/300px-Quicksort-diagram.svg.png)

### 2.1 Lomuto partition scheme

_Introduction to Algorithms_

```
QUICKSORT(A, p, r)
    if p < r
        q = PARTITION(A, p, r)
        QUICKSORT(A, p, q-1)
        QUICKSORT(A, q+1, r)


PARTITION(A, p, r)
    x = A[r]
    i = p - 1
    for j = p to r - 1
        if A[j] <= x
            i = i + 1
            exchange A[i] with A[j]
    exchange A[i+1] with A[r]
    return i + 1
```

```python
# [3,7,8,5,2,1,9,5]     [4]
# [3] [7,8,5,2,1,9,5]   [4]
# [3] [7,8,5] [2,1,9,5] [4]
# [3,2] [8,5,7] [1,9,5] [4]
# [3,2,1] [5,7,8] [9,5] [4]
# [3,2,1] [5,7,8,9,5]   [4]
# [3,2,1] [4] [7,8,9,5,5]
def exchange(A: list, L: int, R: int) -> int:
    pivot = A[R]

    pivot_i = L
    for i in range(L, R):
        if A[i] <= pivot:
            A[pivot_i], A[i] = A[i], A[pivot_i]
            pivot_i += 1

    A[pivot_i], A[R] = A[R], A[pivot_i]
    return pivot_i
```

### 2.2 Hoare partition scheme

![Quicksort-example.gif](https://upload.wikimedia.org/wikipedia/commons/9/9c/Quicksort-example.gif)

_Introduction to Algorithms_

```
QUICKSORT(A, p, r)
    if p < r
        q = PARTITION(A, p, r)
        QUICKSORT(A, p, q)
        QUICKSORT(A, q+1, r)

HOARE-PARTITION(A, p, r)
    x = A[p]
    i = p - 1
    j = r + 1
    while TRUE
        repeat
            j = j - 1
        until A[j] <= x
        repeat
            i = i + 1
        until A[i] >= x
        if i < j
            exchange A[i] with A[j]
        else return j
```

```python
def part(A: list, L: int, R: int):
    if L < R:
        p = exchange(A, L, R)

        part(A, L, p)
        part(A, p+1, R)

# [3,7,8,5,2,1,9,5,4]
# pivot = 5 (actual median)
# [3,4] [8,5,2,1,9,5] [7]
# [3,4,5] [5,2,1] [9,8,7]
#           L,R
# [3,4,5,1] [2] [5,9,8,7]
# [3,4,5,1,2] [5,9,8,7]
# pivot = 3
# [1] [7,8,5,2] [3,9,5,4]
# [1,2] [8,5] [7,3,9,5,4]
#    R   L
# [1,2] [8,5,7,3,9,5,4]
#       [8,5,7,3,9,5,4]
#       [4,5,7,3] [9,5] [8]
#                R   L
#       [4,5,7,3,5] [9,8]
#       [4,5,7,3,5]
#       [3] [5,7] [4,5]
#        R   L
#       [3] [5,7,4,5]
#           [5,7,4,5]
#           [5] [7,4] [5]
#              R   L
#           [5,4] [7,5]
def exchange(A: list, L: int, R: int) -> int:
    pivot = A[L]

    i, j = L-1, R+1
    while i < j:
        #       i       j
        # [..., 5, ..., 5, ...]
        #          i j
        # [..., 5, ..., 5, ...]
        i += 1
        j -= 1

        while i < R and A[i] < pivot:
            i += 1
        while j > L and A[j] > pivot:
            j -= 1

        #       j  i
        # [..., 1, 2, ...]
        #       j  i
        # [..., 2, 1, ...] (wrong)
        if i < j:
            A[i], A[j] = A[j], A[i]
        else:
            return j

    return j
```

### 2.3 Implementation issues

#### 2.3.1 Choice of pivot

#### 2.3.2 Repeated elements

#### 2.3.3 Optimizations

#### 2.3.4 Parallelization

## 3 Formal analysis

### 3.1 Worst-case analysis

### 3.2 Best-case analysis

### 3.3 Average-case analysis

#### 3.3.1 Using percentiles

#### 3.3.2 Using recurrences

#### 3.3.3 Using a binary search tree

### 3.4 Space complexity

## 4 Relation to other algorithms

### 4.1 Selection-based pivoting

### 4.2 Variants

#### 4.2.1 Multi-pivot quicksort

#### 4.2.2 External quicksort

#### 4.2.3 Three-way radix quicksort

[Multi-key quicksort](https://en.wikipedia.org/wiki/Multi-key_quicksort)

#### 4.2.4 Quick radix sort

#### 4.2.5 BlockQuicksort

#### 4.2.6 Partial and incremental quicksort

[Partial sorting](https://en.wikipedia.org/wiki/Partial_sorting)

### 4.3 Generalization

## 5 See also

[Introsort](https://en.wikipedia.org/wiki/Introsort)
