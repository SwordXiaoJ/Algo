Day 39

​	LC62

​		题目链接：[Unique Paths](https://leetcode.com/problems/unique-paths/)

​		解题思路：

​		熟悉套路

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        //1. dp[i][j]表示从（0，0）到（i，j）有dp[i][j]条路径v
        vector<vector<int>> dp(m,vector<int>(n,0));
        
        //2. 递推公式
        //dp[i][j] = dp[i-1][j] + dp[i][j-1]

        //3. 初始化
        for(int i = 0; i < m; i++) dp[i][0] = 1;
        for(int j = 0; j < n; j++) dp[0][j] = 1;

        //4. 遍历顺序,因为是从左和上方进行推导，所以一层一层从左到右遍历就可
        for(int i = 1; i < m; i++)
        {
            for(int j = 1; j < n; j++)
            {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }

        return dp[m-1][n-1];
        
    }
};
```

​	

​	LC63

​		题目链接：[Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

​		解题思路：

​		大致思路与LC62相同，但是注意在初始化的时候，需要判断第一行和第一列初始化的过程中，是否有障碍物，如果有障碍物，则障碍物以后的值都为0。在遍历过程中，如果遇到障碍物,就保持初始状态（初始状态为0）。

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        //dp[i][j]表示从（0，0）到（i，j）有dp[i][j]条路
        vector<vector<int>> dp(m,vector<int>(n,0));
        if(obstacleGrid[0][0] == 1) return 0;

        //确定递归公式
        //dp[i][j] = dp[i-1][j] + dp[i][j-1]

        //初始化
        for(int i = 0; i < m; i++)
        {
            if(obstacleGrid[i][0] == 1) break;
            dp[i][0] = 1;
        }
        for(int j = 0; j < n; j++)
        {
            if(obstacleGrid[0][j] == 1) break;
            dp[0][j] = 1;
        }

        //遍历顺序
        for(int i = 1; i < m; i++)
        {
            for(int j = 1; j < n; j++)
            {
                if(obstacleGrid[i][j] == 1) continue;
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
        
    }
};
```

