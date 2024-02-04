Day 41

​	LC343

​	题目链接：[Integer Break](https://leetcode.com/problems/integer-break/)

​	解题思路：

​	对于正整数 ，可以拆分成至少两个正整数的和。令x是拆分出的第一个正整数，则剩下的部分是 n-x, n-x 可以不继续拆分，或者继续拆分成至少两个正整数的和。由于每个正整数对应的最大乘积取决于比它小的正整数对应的最大乘积，因此可以使用动态规划求解。

​	 在循环中，把一个数拆分的时候，从1开始试，一直试到i-1。所以需要嵌套两个max，找到dp[i]的最大值。

```C++
class Solution {
public:
    int integerBreak(int n) {
        //dp[n]表示拆开数字n，可以得到的最大乘积。
        vector<int> dp(n+1);
        //推导公式
        //当把一个数字拆分的时候，当拆分出来一个数字以后，可以选择继续拆分，也可以选择不拆分
        //dp[i] = max(j*(i-j), j * dp[i-j])
        //初始化
        dp[2] = 1;
        //遍历顺序
        for(int i = 3; i <= n; i++)
        {
            for(int j = 1; j < i ; j++)
            {
                dp[i] = max(dp[i],max(j * (i-j), j * dp[i-j]));
            }
        }
        return dp[n];      
    }
};
```



​	LC96

​		题目链接：[Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

​		解题思路：

​			列出来dp[1],dp[2],dp[3]的样子。

​			发现dp[3],就是1为根节点的BST+2为根节点的BST+3为根节点的BST。

​			1为根节点的BST数量 = 右子树元素为2的子树数量*左子树元素为0的子树数量

​			2为根节点的BST数量 = 右子树元素为1的子树数量*左子树元素为1的子树数量

​			3为根节点的BST数量=  右子树元素为0的子树数量* 左子树元素为2的子树数量

​			dp[3] = dp[2] × dp[0] + dp[1] × dp[1] + dp[0] × dp[2];

​			

```C++
class Solution {
public:
    int numTrees(int n) {
        //dp[i]表示i个节点，可以构建出不同结构的n个BST.
        vector<int> dp(n+1);

        //递推公式(j从0开始，到i-1)
        //dp[i] += dp[j] * dp[i-j-1]

        //初始化
        dp[0] = 1;
       
        for(int i = 1; i <= n; i++)
        {
            for(int j = 0; j <= i-1; j++)
            {
                dp[i] += dp[j] * dp[i-j-1];
            }
        }

        return dp[n];
        
    }
};
```

