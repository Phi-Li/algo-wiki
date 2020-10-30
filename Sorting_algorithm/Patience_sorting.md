<table>
  <tbody>
    <tr><th>Class</th><td>Sorting algorithm</td></tr>
    <tr><th>Data structure</th><td>Array</td></tr>
    <tr><th>Worst-case</th><td><i>O</i>(<i>n</i> log <i>n</i>)</td></tr>
    <tr><th>Best-case</th><td><i>O</i>(<i>n</i>) (pre-sorted input)</td></tr>
  </tbody>
</table>

## 1 Overview

```
  6, 3, 5, 10, 11, 2, 9, 14, 13, 7, 4, 8, 12
  6     5  10  11        14
  3     4   9   8        13
  2         7            12

 (6)
[ 6]

  6
 (3)
[ 3]

  6    (5)
  3
[ 3]

  6     5 (10)
  3
[ 3,    5]

  6     5  10 (11)
  3
[ 3,    5, 10]

  6     5  10  11
  3
 (2)
[ 2,    5, 10, 11]

  6     5  10  11
  3        (9)
  2
[ 2,    5,  9, 11]

  6     5  10  11       (14)
  3         9
  2
[ 2,    5,  9, 11]

  6     5  10  11        14 
  3         9           (13)
  2
[ 2,    5,  9, 11        13]

  6     5  10  11        14 
  3         9            13
  2        (7)
[ 2,    5,  7, 11        13]

  6     5  10  11        14 
  3    (4)  9            13
  2         7
[ 2,    4,  7, 11        13]

  6     5  10  11        14 
  3     4   9  (8)       13
  2         7
[ 2,    4,  7,  8        13]

  6     5  10  11        14 
  3     4   9   8        13
  2         7           (12)
[ 2,    4,  7,  8        12]
```

### 1.1 Analysis

1. Patience game simulation, binary search, *O*(*n* log *n*).
2. K-way merge, priority queue, *O*(*n* log *n*).

## 2 Relations to other problems

### 2.1 Algorithm for finding a longest increasing subsequence

## 3 History

## 4 Use
