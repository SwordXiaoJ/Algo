Day 55

​	LC309

​		题目链接：[Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

​		解题思路：

   1. dp数组定义

      ```
      dp[i][j] 表示第i天，状态为j，所剩余的最多现金为dp[i][j]
      
      j为以下状态
      0为持有股票状态（之前买入且没有任何操作，或者当天买入）
      1为不持有股票状态（之前卖出）
      2为不持有股票，当天卖出,这说明后一天就是冷冻期了(这个状态单独出来的原因是因为涉及到冷冻期)
      3为冷冻期，注意冷冻期仅仅一天，不可以持续
      ```

      2为不持有股票，当天卖出,这说明只有就是冷冻期了(这个状态单独出来的原因是因为涉及到冷冻期) 是单独出来的。因为有冷冻期

   2. 递推公式

      状态0：

      ​	(1) 前一天就是持有股票。

      ​	(2) 当天买入

      ​		<1> 前一天是冷冻期

      ​		<2> 前一天不持有股票（之前卖出），前一天没有卖出操作。状态1

      ```C++
      dp[i][0] = max(dp[i-1][0],dp[i-1][3] - prices[i],dp[i-1][1] - prices[i])
      ```

      状态1：

      ​	（1） 前一天就不持有（之前卖出）

      ​	（2）前一天是冷冻期

      ​	

      ```
      dp[i][1] = max(dp[i-1][1],dp[i-1][3]);
      ```

      状态2：

      ​	（1）之前持有股票，当天卖出

      ```
      dp[i][2] = dp[i-1][0] + prices[i];
      ```

      状态3：

      ```
      dp[i][3] = dp[i-1][2];
      ```

      ​	

   3. 初始化

      ```
      dp[0][0] = -prices[i];
      dp[0][1] = 0;
      dp[0][2] = 0;
      dp[0][3] = 0;
      ```

      

   4. 从前往后遍历

      

```C++ 
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<vector<int>> dp(prices.size(),vector<int>(4,0));
        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        dp[0][2] = 0;
        dp[0][3] = 0;
        for(int i = 1; i < prices.size(); i++)
        {
            dp[i][0] = max(dp[i-1][0],max(dp[i-1][3] - prices[i], dp[i-1][1] - prices[i]));
            dp[i][1] = max(dp[i-1][1],dp[i-1][3]);
            dp[i][2] = dp[i-1][0] + prices[i];
            dp[i][3] = dp[i-1][2];
        }
        return max(max(dp[prices.size()-1][0],dp[prices.size()-1][1]),max(dp[prices.size()-1][2],dp[prices.size()-1][3]));
    }
};
```



​	LC714

​		题目链接：[ Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

​		解题思路：

1. dp数组

   dp[i] [0] 表示第i天持有股票的最多现金

   dp[i] [1] 表示第i天不持有股票的最多现金

2.  递推公式

   ```
   dp[i][0] = max(dp[i-1][0],dp[i-1][1] - prices[i];
   
   dp[i][1] = max(dp[i-1][1],dp[i-1][0] + prices[i] - fee);
   //多了一个手续费
   ```

   ```C++
   class Solution {
   public:
       int maxProfit(vector<int>& prices, int fee) {
           vector<vector<int>> dp(prices.size(),vector<int>(2,0));
           dp[0][0] = -prices[0];
           for(int i = 1; i < prices.size(); i++)
           {
               dp[i][0] = max(dp[i-1][0],dp[i-1][1] - prices[i]);
   
               dp[i][1] = max(dp[i-1][1],dp[i-1][0] + prices[i] - fee);
           }
   
           return max(dp[prices.size()-1][0],dp[prices.size()-1][1]);
           
       }
   };
   ```

   
