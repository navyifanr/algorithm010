# 学习笔记
--------------

### 字典树

> 字典树，即 Trie 树，又称单词查找树或键树，是一种树形结构。典型应用是用于统计和排序大量的字符串(但不仅限于字符串)，所以经常被搜索引擎系统用于文本词频统计。
它的优点是: **最大限度地减少无谓的字符串比较，查询效率比哈希表高。**

核心思想: Trie 树的核心思想是空间换时间。利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。


### 并查集

> 并查集是一种树型的数据结构，用于处理一些不交集（Disjoint Sets）的合并及查询问题。有一个联合-查找算法（union-find algorithm）定义了两个用于此数据结构的操作：
  Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
  Union：将两个子集合并成同一个集合。

```java
class UnionFind {
    private int count = 0;
    private int[] parent;

    public UnionFind(int n) {
        count = n;
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }

    public int find(int p) {
        while (p != parent[p]) {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }

    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ) return;
        parent[rootP] = rootQ;
        count--;
    }
}
```

[Union-Find算法详解](https://labuladong.gitbook.io/algo/gao-pin-mian-shi-xi-lie/unionfind-suan-fa-xiang-jie)


### 高级搜索

- 剪枝
- 双向 BFS 
- 启发式搜索(A*)


### 高级树、AVL 树和红黑树

#### 二叉搜索树 Binary Search Tree

> 二叉搜索树，也称二叉搜索树、有序二叉树(Ordered Binary Tree)、排序二叉树(Sorted Binary Tree)，是指一棵空树或者具有下列性质的二叉树:
1. 左子树上所有结点的值均小于它的根结点的值;
2. 右子树上所有结点的值均大于它的根结点的值;
3. 以此类推:左、右子树也分别为二叉查找树。 (这就是 重复性!)
中序遍历:升序排列

#### AVL 树
> 一种平衡(balanced)的二叉搜索树(binary search tree, 简称为BST)
1. 发明者 G. M. Adelson-Velsky 和 Evgenii Landis
2. Balance Factor(平衡因子): 是它的左子树的高度减去它的右子树的高度(有时相反)。 balancefactor={-1, 0, 1}
3. 通过旋转操作来进行平衡(四种)

[彻底搞懂AVL树](https://www.jianshu.com/p/65c90aa1236d)

#### 红黑树 Red-black Tree

> 红黑树是一种近似平衡的二叉搜索树(Binary Search Tree)，它能够确保任何一个结点的左右子树的高度差小于两倍。具体来说，红黑树是满足如下条件的二叉搜索树:
• 每个结点要么是红色，要么是黑色
• 根结点是黑色
• 每个叶结点(NIL结点，空结点)是黑色的。
• 不能有相邻接的两个红色结点
• 从任一结点到其每个叶子的所有路径都包含相同数目的黑色结点。

[30张图带你彻底理解红黑树](https://www.jianshu.com/p/e136ec79235c)