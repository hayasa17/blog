+++
date = '2024-11-15T17:02:05+08:00'
draft = false 
title = 'DataStructure'
+++
## Balanced Tree & Search Tree

先明确几个概念
- 高度：空树的高度被视作１
- 平衡：左右子树的高度差不超过１
- 旋转：分为左旋和右旋，详情见图

### AVL Tree 

这是最早接触的平衡树，要求绝对的平衡．

#### rotation 
每次加入或删除节点后通过旋转来保持平衡．旋转种类分为LR,LL,RL,RR

简而言之，就是将不平衡的节点需要旋转的部分，先旋转为一条直链，再进行旋转，若一开始便为直链则不需要．

原因：TBD

#### 复杂度
单次时间复杂度严格$O(log n)$

### Splay Tree

每次操作都会将访问的节点旋转至根节点，称为access.

#### 基本操作

核心操作:access

旋转操作分为两种，zigzig 和 zigzag

zigzag 与 AVL Tree中的旋转相同，处理折链情况．

zigzig 如图

特别的．如果当先 access 的节点已经为 root 的儿子，则简简单单的将其转到 root 上

- Insert: 直接在二叉搜索树上寻找到位置插入，并且 access

- Merge: 要求其中一个树的最小值大于另一棵树的最大值,access 小树的最大值，将大树接在小树的右儿子上

- Delete: access 所删节点,　删去根节点，然后 Merge 左右子树
PS: Merge 同样可以 access 大树的最小值，但是与 ppt 不同

#### 复杂度

均摊时间复杂度$O(log n)$，也就是说 n 次连续操作时间复杂度$O(n log n)$

#### 重要原句

worst-case bound >= amortized bound >= average-case bound

Splaying not only moves the accessed node to the root, but also roughly halves the depth of most nodes on the path.

### Analysis

#### Amortized Analysis

势能分析法

splay 的复杂度分析：TBD

#### Aggregate analysis

#### Accounting method


### Reb-black Tree

- 节点为红色或黑色
- 根节点为黑色
- NIL为黑色
- 红节点的儿子都为黑色
- bh(black height)相同



#### Insert
插入的节点默认为红色节点，会在后续操作中重新染色

Case1:

Case2:

Case3:

#### delete

删除操作可以通过一些操作进行简化，删除后还需要进行重新染色保证性质５
- 若删除的为叶子节点，则直接删除
- 若删除的节点度数为１，则用其孩子代替他
- 若删除的节点度数为２，则先将其与其前驱或后继替换（仅交换数值），再进行删除

重新染色：

Case1:

Case2:

Case3:

Case4:




#### 重要结论

有n个内部节点的红黑树深度最多为$2ln(n+1)$

证明：
- $sizeof(x) >= 2^(bh(x))-1$,可用归纳法证明
- $bh(tree)>=h(tree)/2$,可用反证法证明
- 结合以上两点可以证明

|  旋转次数 | AVL | RBTree |
| ------ | ------ | ------ |
| Insertion | $<=2$ | $<=2$|
| Deletion | $O(log n)$ |$<=3$ |

红黑树的recolor次数仍是$O(log n)$
### B+ Tree

B+树实际上是一个特殊的多叉搜索树

B+树有一个关键参数M，当M为4时成为2-3-4树，当M为3时成为2-3树
- 根节点是一个叶子或有２~M个儿子
- 非叶子节点（除了根）有$\lceil M/2 \rceil$ ~ M 儿子
- 所有叶子深度相同

特别的:The best choice of M is 3 or 4.

图例最为直观
#### Insertion
按照搜索树的性质找到插入的节点进行插入，若插入后导致该节点内的数据过多时，则将该节点分裂为$\lceil (M+1)/2 \rceil$ 和　$\lfloor (M+1)/2 \rfloor$两个节点，同时更新父亲的索引值，类似操作递归至根节点．

PS:分裂的两个节点顺序要与文中相同

#### Deletion

类似Insertion的逆操作，当儿子删得仅剩一个时要删除父节点．

ppt中没有过多描述．

## Inverted File Index

正常来讲，文章中包含了一个个单词.
所谓Inverted File Index，Inverted体现在于在单词为索引的Index中存下了文章．

具体而言就是记录下每个单词出现在文章的位置

###　术语
该知识点比较简单，下面仅列举一些术语:

- Word Stemming: 词干提取，提取出单词的词根
- Stop Words: 忽略一些常用无实意词如a,the,it


| |Relevant| Irrelevant|
| ----- | ------ | ----- |
| Retrieved| $R_R$ | $I_R$|
| Not Retrieved| $R_N$ | $I_N$|

Precision P = $R_R /(R_R +I_R)$
用来衡量查询结果是否准确

Recall R=$R_R /(R_R+R_n)$
用来衡量查询结果是否充分

### 可能重要的原句：
Most gaps can be encoded with far fewer than 20 bits.

Gap 指的是在存储Inverted File Index时，存储一个term的列表是选择存储其差分列表的列表内最大值．


## Heap

以下的Heap都是以Merge操作为核心的Heap

### Leftist Heap

NPL: 即NULL Path Length，即到自己一个非满儿子的后代的距离

左偏堆，顾名思义向左倾斜的堆，实际上为左边的NPL更大的堆
#### 性质 

A leftist tree with r nodes on the right path must have at least $2^r-1 $ nodes.

比较生动的理解就是，把深度大于$r$的节点全部砍掉，剩下的会是一颗满二叉树.
这点由NPL的定义很好推出.

#### 基本操作

- Merge: 抽象来说，将rightpath上的边全部剪断，将所得森林重新排序构成rightpat，重构的过程实际上是一个归并的过程，而且rightpath长度不超过 $log n$,所以复杂度为$O(log n)$. 之后重新对rightpath上的点进行NPL要求的维护.
- Insert: 仅有root的LeftistHeap的Merge
- Delete Min: 删去根节点后，Merge根节点的左右儿子

综上所有操作为$O(logn)$
### Skew Heap

Leftist Heap和Skew Heap的关系类似于AVL Tree 和 Splay Tree 的关系

正如Splay相较于AVL没有，Skew Heap橡胶Leftist Heap 没有NPL的概念
#### 基本操作

- Merge: 类似于Leftist，但是没有对NPL的维护，而是在每次重构时都交换左右儿子
- Insert: 仅有root的SkewHeap的Merge
- Delete Min: 删去根节点后，Merge根节点的左右儿子

#### 复杂度分析
利用Amotized Analysis
TBD

均摊$O(logn)$

