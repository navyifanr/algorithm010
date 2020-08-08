学习笔记
--------------------

## 动态规划

动态规划和递归或者分治，没有根本上的区别 (关键看有无最优的子结构)

共性:找到重复子问题 
差异性:最优子结构、中途可以淘汰次优解


求解动态规划的核心问题是穷举。

- 但动态规划的穷举有点特别，因为这类问题存在「重叠子问题」，如果暴力穷举的话效率会极其低下，所以需要「备忘录」或者「DP table」来优化穷举过程，避免不必要的计算；
- 动态规划问题一定会具备「最优子结构」，才能通过子问题的最值得到原问题的最值；
- 穷举所有解并不是一件简单的事情，一般要借助**状态转移方程**，才能正确穷举；

思考状态转移方程思路：
**明确 base case -> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义。**

对应的框架：
```text
# 初始化 base case
dp[0][0][...] = base
# 进行状态转移
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 求最值(选择1，选择2...)
```

[63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)
状态方程：
```
if obstacleGrid[i][0] != 1
    dp[i][0] = 1
else dp[i..end][0] = 0
if obstacleGrid[0][j] != 1
       dp[0][j] = 1
   else dp[0][j..end] = 0
if obstacleGrid[i][j] == 1
    dp[i][j] = 0
else if !(obstacleGrid[i-1][j]==1 && obstacleGrid[i][j-1]==1)
    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
```


## 字符串

[字符串匹配的KMP算法](https://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)

