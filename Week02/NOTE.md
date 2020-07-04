学习笔记
-------------

### 1. 哈希表

> 哈希表(Hash table)，也叫散列表，是根据关键码值(Key value)而直接进行访问的数据结构。 它通过把关键码值映射到表中一个位置来访问记录，以加快查找的 速度。
  这个映射函数叫作散列函数(Hash Function)，存放记录的数组 叫作哈希表(或散列表)。

Map: key-value 键值对，key 不可重复，value可重复；HashMap, HashTable, ConcurrentHashMap
Set: 不重复元素；HashSet, TreeSet


HashMap 和 HashSet 总结：

HashMap 源码简析：


put 方法:

- 对 key 的 hashcode 做 hash 值；
- 哈希表为空时，创建新的；找到要插入的下标，如果对应值为空，则直接插入；
- 如果发生哈希碰撞：
    - 如果节点的 key 和插入的一致，替换原来值
    - 如果节点结构是树结构，则以红黑树插入
    - 如果是链表结构，找到对应要插入的节点，如果链表过长，则转为红黑树，否则节点不存在，插到队尾，存在则替换；
```$java
    public V put(K key, V value) {
        //对 key 的 hashcode 做 hash 
        return putVal(hash(key), key, value, false, true);
    }

    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        //哈希表为空时，创建新的
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        //通过hash找到下标，如果下标对应的节点值为空，则将数据存储进去
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else { //通过hash找到的下标，有数据，发生碰撞
            Node<K,V> e; K k;
            //找到的下标的 key 和要插入的一致，将 e 赋值为 p
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            //找到的下标的 key 和要插入的不一致，桶中数据是树结构，通过红黑树插入
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            //桶中数据是链表结构
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        //链表中不存在要插入的节点，则放在链表队尾
                        p.next = newNode(hash, key, value, null);
                        //链表过长，超过阈值，则转为红黑树
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    //链表中存在要插入的节点，替换调原来的，跳出循环
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            //更新发生冲突的键值对
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        //hashmap size 大于阈值，则扩容
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

get 方法:

- 直接根据 hash 值找对应节点数据；
- 如果节点的 key 一致，则直接返回；不一致，则判断节点是树结构还是链表结构，查询返回

```$java
    public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }

    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            //根据hash值，找到节点，key一致，则直接返回对应的节点
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                //节点为红黑树，则通过树结构查找
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    //节点为链表，遍历查询
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
```


面试解题四步法：
1. 和面试官过一遍题目；
2. 从中找到一个最优的解法(时间&&空间复杂度最优)
3. write code
4. test cases

PS: 如果考察的不是排序题，可以直接调系统排序方法

**收集优质的解题方法**


### 2. 树、二叉树、二叉搜索树

树：由n（n>0）个有限节点组成一个具有层次关系的集合；
二叉树(binary)：一种特殊的树。二叉树的每个节点最多只能有2个子节点；
二叉搜索树：也称二叉搜索树、有序二叉树(Ordered Binary Tree)、 排序二叉树(Sorted Binary Tree)，是指一棵空树或者具有下列性质的 二叉树:
1. 左子树上所有结点的值均小于它的根结点的值;
2. 右子树上所有结点的值均大于它的根结点的值;
3. 以此类推:左、右子树也分别为二叉查找树。 

可以理解为：Linked List 是特殊化的 Tree, Tree 是特殊化的 Graph

链表查询加速在于升维，比如改为跳表
树和图的差别在于有没有环
树的面试题一般都是递归

二叉树的遍历方式：
- 前序(Pre-order):根-左-右 
- 中序(In-order):左-根-右 
- 后序(Post-order):左-右-根

**二叉树操作流程图示**：https://visualgo.net/zh/bst
时间复杂度：http://www.bigocheatsheet.com/


### 3. 堆、二叉堆

堆（Heap）:可以快速找到一堆数据中最大或最小值的数据结构；
> 将根节点最大的堆叫做大顶堆或大根堆，根节点最小的堆叫做小顶堆或小根堆。常见的堆有**二叉堆、斐波那契堆**等。

不同类型的堆：https://en.wikipedia.org/wiki/Heap_(data_structure)  
二叉堆是堆中比较常见而且简单的实现，但效率并不是最优的；

二叉堆：通过完全二叉树来实现(注意:不是二叉搜索树) ;
二叉堆(大顶)它满足下列性质:
[性质一]是一棵完全树
[性质二]树中任意节点的值总是>=其子节点的值;

特点：
1. 二叉堆一般都通过“数组”来实现
2. 假设"第一个元素"在数组中的索引为0的话，
则父节点和子节点的位置关系如下:
(01)索引为i的左孩子的索引是(2*i+1);
(02)索引为i的右孩子的索引是(2*i+2);
(03)索引为i的父结点的索引是flor((i-1)/2);


### 4. 图

图是一种复杂的非线性结构。
- 属性：节点(Vertex，入度出度) 与 边（Edge，有向无向和权重）
- 图的表示： 邻接表 和 邻接矩阵
- 分类：方向-有向图和无向图，权重-有权图和无权图
- 图的遍历： DFS BFS 
- 常见可以解决的问题:
> 联通分量 Flood Fill 寻路 走迷宫 迷宫生成 无权图的最短路径 环的判断
最小生成树问题（Minimum Spanning Tree） Prim Kruskal
最短路径问题(Shortest Path) Dijkstra Bellman-Ford
拓扑排序(Topological sorting)




