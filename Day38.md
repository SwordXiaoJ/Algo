Day 38

​	DP(如果某一个问题有很多个重叠的子问题，那么DP是最有效的)

​	动态规划 VS 贪心

​	  动态规划中每一个状态是由上一个状态推导出来的。贪心没有状态推导，直接是从局部选取最优的。

​	DP的解题套路：
		1. 确定DP数组以及下标含义。
		1. 确定推导公式
		1. DP数组初始化
		1. 确定遍历顺序
		1. 举例推导





​	LC509

​		题目链接：[Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

​		一次AC，没什么好说的，熟悉套路



​	LC70

​		题目链接：[Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

​		解题思路：

   			1. dp[i]表示上到第i层，有dp[i]种方法
   			2. dp[i] = dp[i-1] + dp[i-2] , 因为上到第i-1层，一步一个台阶就可以到第i层。上到i-2层，一步两个台阶就可以到第i层。
   			3. dp[1] = 1, dp[2] = 2;
   			4. 根据推导公式，所以是从前往后遍历。
   			5. 举例模拟

```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 2) return n;
        //dp[i]表示爬到第i层，有dp[i]种方法
        vector<int> dp(n+1);
        //推导公式
        //dp[i] = dp[i-1] + dp[i-2]
        //初始化
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i <= n; i++)
        {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
        
    }
};
```

​	LC746

​		题目链接：[Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

​		一次AC

​		这道题值得注意的是因为是要求爬到顶层，所以其实在cost的长度之外，其实还有一层。所以在dp的时候，它的长度是cost.size() + 1。 在最后返回的时候，也返回的是dp[cost.size()]。这里要注意到下标和长度的关系。cost.size()仅仅表示这个数量的楼梯，顶层还有一层。

楼梯顶部实际上是在最后一阶楼梯之后的位置。因此，如果楼梯有 `n` 阶（`cost.size() == n`），楼梯顶部将是第 `n+1` 阶，即 `dp[n]`。

```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        //dp[i]表示爬到第i级楼梯，你需要花费的最少精力为dp[i]
        vector<int> dp(cost.size()+1);
        //递推公式
        //dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2]);
        //初始化
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2; i <= cost.size(); i++)
        {
            dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2]);
        }
        return dp[cost.size()];
        
        
    }
};
```

​		
