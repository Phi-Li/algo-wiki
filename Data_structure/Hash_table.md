# Hash table

__hash table__ (__hash map__)

A hash table uses a _hash function_ to compute an _index_, also called a _hash code_, into an array of _buckets_ or _slots_, from which the desired value can be found.

_hash collision_

## 1 Hashing

__P4__ 在数组大小是2的幂的情况下，余数操作被简化为掩码，这提高了速度，但会增加哈希函数不佳时带来的问题。

### 1.1 Choosing a hash function

__P3__ 对于开放寻址法，散列函数也应避免 _clustering_，即两个或多个键映射到连续的 slot 。即使 load factor 很低， collisions 也不频繁，这种 clustering 仍可能会导致查找成本激增（to skyrocket）。 The popular multiplicative hash is claimed to have particularly poor clustering behavior.

### 1.2 Perfect hash function

## 2 Key statistics

_load factor_

```
               n - entries occupied in the hash table
load factor = ---
               k - buckets
```

## 3 Collision resolution

### 3.1 Separate chaining

#### 3.1.1 Separate chaining with linked lists

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Hash_table_5_0_1_1_1_1_1_LL.svg/450px-Hash_table_5_0_1_1_1_1_1_LL.svg.png)

__P6__ 另外一个缺点是，遍历一个链表的缓存性能很差，使得处理器的缓存没有效果。

#### 3.1.2 Separate chaining with list head cells

![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Hash_table_5_0_1_1_1_1_0_LL.svg/500px-Hash_table_5_0_1_1_1_1_0_LL.svg.png)

__P1__ 一些链式实现将每条链的第一条记录存储在 slot array 本身。在大多数情况下，指针遍历的次数会减一。其目的是为了提高哈希表访问的缓存效率。

#### 3.1.3 Separate chaining with other structures

__P1__ 例如，通过使用自平衡二叉搜索树，常见的哈希表操作（插入、删除、查找）的理论最坏情况下的时间可以减少到 O(log n)。然而，这在实现中引入了额外的复杂性，对于较小的哈希表来说，这可能会导致更糟糕的性能，因为插入和平衡树结构的时间大于线性地搜索列表中所有的元素所需的时间。

__P2__ _数组哈希表_ 使用一个 _动态数组_ 来存储所有哈希到同一 slot 的条目。每个新插入的条目都被追加到分配给 slot 的动态数组的末尾。动态数组是以 _exact-fit_ 的方式调整大小的，这意味着它只按需要的字节数增长。其他方式，如通过块大小或*页*来增长数组，可以提高插入性能，但要付出空间上的代价。数组哈希表更有效地利用了 CPU 缓存和 translation lookaside buffer (TLB)，因为 slot 条目被存储在连续的内存位置。它还省去了链接列表所需的下一个指针，从而节省了空间。尽管经常调整数组的大小，操作系统所引入的空间开销，如内存碎片，实际上很小。

__P3__ _动态完美散列_，一个包含 _k_ 个条目的 bucket 被组织成一个具有 _k^2_ slots 的完美散列表。虽然它占用了更多的内存（在最坏的情况下，_n_ 个条目有 _n^2_ slots，在平均情况下有 _n_ × _k_ slots），但这种变体保证了最坏情况下恒定的查找时间，以及插入的低均摊时间。也可以为每个桶用一棵 _fusion tree_，来实现所有操作高概率的恒定时间。

### 3.2 Open addressing

_probe sequence_

1. Linear probing (线性探测), 探测的间隔是固定的（通常是1）。由于良好的CPU缓存利用率和高性能，这种算法在现代计算机架构上的哈希表实现中得到了最广泛的应用。
2. Quadratic probing (平方探测), 初始的哈希计算所给的起始值加上二次多项式的序列输出，从而增加探测之间的间隔。
3. Double hashing (再散列), 探测之间的间隔由另一个散列函数计算。

![](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Hash_table_5_0_1_1_1_1_0_SP.svg/380px-Hash_table_5_0_1_1_1_1_0_SP.svg.png)
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; ![](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1c/Hash_table_average_insertion_time.png/362px-Hash_table_average_insertion_time.png)

__P7__ 另外，普通的开放寻址对于大的元素来说是一个糟糕的选择，因为这些元素会填满整个的缓存行（失去了缓存的优势），大的空表槽浪费大量的空间。如果表只存储对元素的引用（元素存储在外部），则会失去速度优势。

__P8__ 一般来说，开放寻址更适用于具有小记录的哈希表，这些小记录可以存储在表内（内部存储）并能装入缓存行中。它们特别适用于一个字或更小的元素。如果哈希表预计会有很高的 load factor ，记录很大，或者数据是可变大小的，链式哈希表往往能表现得同样好或更好。

#### 3.2.1 Coalesced hashing

[Coalesced hashing - Wikipedia](https://en.wikipedia.org/wiki/Cuckoo_hashing)

#### 3.2.2 Cuckoo hashing

[Cuckoo hashing - Wikipedia](https://en.wikipedia.org/wiki/Cuckoo_hashing)

#### 3.2.3 Hopscotch hashing

[Hopscotch hashing - Wikipedia](https://en.wikipedia.org/wiki/Hopscotch_hashing)

#### 3.2.4 Robin Hood hashing

再散列的一个变体是 Robin Hood 散列。

如果一个新的 key 的探查计数大于当前位置已经插入的 key 的探查计数，那么它可以置换这个 key 。这样做的净效果是，它减少了最坏情况下的搜索时间。这与有序哈希表相似，只是 bump 一个键的标准不取决于键之间直接的关系。

由于最坏情况和探测次数的变化都大大减少，一个有趣的变体是从预期的成功探测值开始探测表，然后从这个位置双向扩展。

External Robin Hood hashing 是这种算法的扩展，其中表被存储在一个外部文件中，每个表的位置对应一个固定大小的页面或有 _B_ 条记录的桶。

### 3.3 2-choice hashing

## 4 Dynamic resizing

_rehash_

### 4.1 Resizing by copying all entries

### 4.2 Alternatives to all-at-once rehashing

#### 4.2.1 Incremental resizing

渐进地 rehash:

1. 在 resize 期间，分配新的哈希表，但保持旧表不变。
2. &emsp;
	- 每次查找或删除，都检查这两个表。
	- 仅在新表中插入。
	- 每次插入时，将 _r_ 个元素从旧表移到新表。
3. 当所有元素都从旧表中移出时，删除它。

为了确保旧表能在新表本身需要扩容之前被完全复制过来，有必要在 resizing 时将表的大小乘至少 (_r_ + 1) / _r_ 的系数。

#### 4.2.2 Monotonic keys

如果已知 keys 将以单调递增（或递减）的顺序存储，就可以实现一致性散列的一种变体。

给定某个初始 key _k1_，它之后的 key _ki_ 将整个 key domain [_k1_, ∞) 划分为集合 {[_k1_, _ki_), [_ki_, ∞)}。对某个单调递增的 key 序列 (_ki0_, ..., _kin_) (_n_ 是细分的数量)，重复这个过程可以得到一个粒度更细的分区 {[_k1_, _ki0_), [_ki0_, _ki1_), ..., [_kin-1_, _kin_), [_kin_, ∞)}。同样的过程也适用于单调递减的 key 。通过给这个分区的每个子区间分配一个不同的哈希函数或哈希表（或两者），以及每当哈希表扩容时对分区进行细化，这种方法保证了即使哈希表在扩大，任何 key 的哈希值，一旦确定，将永远不会改变。

由于常常倍增总条目数，所以只有 O(log(_N_)) 个子区间需要检查，重定向的二分搜索时间为 O(log(log(_N_))。

#### 4.2.3 Linear hashing

线性散列是一种允许散列表增量扩展的散列表算法。它使用单个哈希表来实现，但有两个可能的查找函数。

#### 4.2.4 Hashing for distributed hash tables

另一种降低表 resizing 成本的方法是选择一个哈希函数，使大多数值的哈希值在表 resizing 时不变。

这样的哈希函数在磁盘上的哈希表和分布式哈希表中很普遍，在这种情况下， rehashing 的成本过高。

设计一个哈希值，使大多数值在表被调整大小时不变，这个问题被称为分布式哈希表问题。

四种最通行的方法是：
1. Rendezvous hashing
2. Consistent hashing
3. Content addressable network algorithm
4. Kademlia distance

------

[Cache Performance in Hash Tables with Chaining vs Open Addressing](https://stackoverflow.com/questions/49709873)
