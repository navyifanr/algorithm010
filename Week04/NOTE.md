学习笔记
------------

### BFS && DFS

广度优先搜索 BFS

代码模板：
```
//1. 如果不需要确定当前遍历到了哪一层，BFS模板
while queue 不空：
    cur = queue.pop()
    for 节点 in cur的所有相邻节点：
        if 该节点有效且未访问过：
            queue.push(该节点)
//2. 如果要确定当前遍历到了哪一层，BFS模板
level = 0
while queue 不空：
    size = queue.size()
    while (size --) {
        cur = queue.pop()
        for 节点 in cur的所有相邻节点：
            if 该节点有效且未被访问过：
                queue.push(该节点)
    }
    level ++;
```

深度优先搜索 DFS

代码模板：
```
//递归
visited = set()
def dfs(node, visited):
    if node in visited: # terminator
    	# already visited
    	return
	visited.add(node)
	# process current node here.
	...
	for next_node in node.children():
		if next_node not in visited:
			dfs(next_node, visited)

//非递归
def DFS(self, tree):
	if tree.root is None:
		return []
	visited, stack = [], [tree.root]
	while stack:
		node = stack.pop()
		visited.add(node)
		process (node)
		nodes = generate_related_nodes(node)
		stack.push(nodes)
	# other processing work
	...
```

### 贪心算法
    
> 贪心算法是一种在每一步选择中都采取在当前状态下最好或最优(即最有利)的选择，从而希望导致结果是全局最好或最优的算法。
  贪心算法与动态规划的不同在于**它对每个子问题的解决方案都做出选择，不能回退**。动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。


### 二分查找

代码模板
```
int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;

    while(left <= right) {
        int mid = left + (right - left) / 2;  //防溢出
        if (nums[mid] == target) {
            //找到目标值
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        }
    }
    return ...;
}
```
注意，以上是基础的二分查找，当区间是 [left, right]时，while 循环比较时包含等号，否则如果是 [left, right)时，while 循环是不包含等号的

以下是变形的情况：参考 [二分查找详解](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/er-fen-cha-zhao-xiang-jie)
```
1. 寻找左侧边界的二分查找
因为我们初始化 right = nums.length
所以决定了我们的「搜索区间」是 [left, right)
所以决定了 while (left < right)
同时也决定了 left = mid + 1 和 right = mid

因为我们需找到 target 的最左侧索引
所以当 nums[mid] == target 时不要立即返回
而要收紧右侧边界以锁定左侧边界

2. 寻找右侧边界的二分查找
因为我们初始化 right = nums.length
所以决定了我们的「搜索区间」是 [left, right)
所以决定了 while (left < right)
同时也决定了 left = mid + 1 和 right = mid

因为我们需找到 target 的最右侧索引
所以当 nums[mid] == target 时不要立即返回
而要收紧左侧边界以锁定右侧边界

又因为收紧左侧边界时必须 left = mid + 1
所以最后无论返回 left 还是 right，必须减一

```


问题：使用二分查找，寻找一个半有序数组 [4, 5, 6, 7, 0, 1, 2] 中间无序的地方

思路：半有序（升序）的数组，前半段的值肯定大于后半段的值；按常规的二分查找，
  1. 如果中间值大于 left 值，那前半段有序，无序地方在后半段，并且 left = mid，因为无序的地方可能是 mid 开始；
  2. 如果中间值小于 left 值，那后半段有序，无序地方在前半段，并且 right = mid，无序的地方也可能 mid 开始；
  3. 结束条件是当中间值等于 left 值的时候，最终返回 left，right，就是中间无序的地方
  
```java
    public int[] findDisorderPosition(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while (left < right) {
            int mid = (left + right) / 2;
            if (nums[mid] == nums[left]) {
                return new int[]{nums[left], nums[right]};
            } else if (nums[mid] > nums[left]) {
                left = mid;
            } else {
                right = mid;
            }
        }
        return new int[]{};
    }
```





