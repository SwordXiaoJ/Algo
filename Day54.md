Day 54

​		LC123

​		题目链接：[Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
​		解题思路：

   1. 确定dp数组以及含义

      一天一共就有五个状态，

      0. 没有操作 （其实我们也可以不设置这个状态）

      1. 第一次持有股票(当天买入，或者延续之前的状态)

      2. 第一次不持有股票

      3. 第二次持有股票

      4. 第二次不持有股票

         dp[i] [j]中 i表示第i天，j为 [0 - 4] 五个状态，dp[i][j]表示第i天状态j所剩最大现金。

         需要注意：dp[i] [1]，**表示的是第i天，买入股票的状态，并不是说一定要第i天买入股票，这是很多同学容易陷入的误区**。

   2. 递推公式

      ```
      dp[i][1] = max(dp[i-1][1],dp[i-1][0] - prices[i]);
      dp[i][2] = max(dp[i-1][2],dp[i-1][1] + prices[i]);
      dp[i][3] = max(dp[i-1][3],dp[i-1][2] - prices[i]);
      dp[i][4] = max(dp[i-1][4],dp[i-1][3] + prices[i]);
      ```

   3. 初始化

      ```
      dp[0][0] = 0;
      dp[0][1] = -prices[0];
      dp[0][2] = 0; // 当天买入，当天卖出
      dp[0][3] = -prices[0];
      dp[0][4] = 0;
      ```

      

   4.  遍历顺序，从前往后

​      

   5. ```C++
      class Solution {
      public:
          int maxProfit(vector<int>& prices) {
              vector<vector<int>> dp(prices.size(),vector<int>(5,0));
              dp[0][0] = 0;
              dp[0][1] = -prices[0];
              dp[0][2] = 0; // 当天买入，当天卖出
              dp[0][3] = -prices[0];
              dp[0][4] = 0;
              for(int i = 1; i < prices.size(); i++)
              {
                  dp[i][0] = dp[i-1][0];
                  dp[i][1] = max(dp[i-1][1],dp[i-1][0] - prices[i]);
                  dp[i][2] = max(dp[i-1][2],dp[i-1][1] + prices[i]);
                  dp[i][3] = max(dp[i-1][3],dp[i-1][2] - prices[i]);
                  dp[i][4] = max(dp[i-1][4],dp[i-1][3] + prices[i]);
              }
              return dp[prices.size()-1][4];
              
          }
      };
      ```

      6. 空间优化写法

         每一个状态，只依赖于前一轮，所以就只需要存储前一轮状态就好。不需要保存所有的状态。

         ```C++
         class Solution {
         public:
             int maxProfit(vector<int>& prices) {
                 if (prices.size() == 0) return 0;
                 vector<int> dp(5, 0);
                 dp[1] = -prices[0];
                 dp[3] = -prices[0];
                 for (int i = 1; i < prices.size(); i++) {
                     dp[1] = max(dp[1], dp[0] - prices[i]);
                     dp[2] = max(dp[2], dp[1] + prices[i]);
                     dp[3] = max(dp[3], dp[2] - prices[i]);
                     dp[4] = max(dp[4], dp[3] + prices[i]);
                 }
                 return dp[4];
             }
         };
         ```

         

​		总结：

### 最多两次交易的情况

在这种情况下，我们的目标是在限定最多进行两次买卖的条件下最大化利润。这意味着我们的操作（买入和卖出）是受限的，且第二次操作依赖于第一次操作的结果。具体来说：

- **第一次买入**后，我们的资金减少了。
- **第一次卖出**后，我们获得了利润。
- **第二次买入**时，我们可以使用之前获得的利润，这意味着第二次买入的能力受到第一次卖出利润的影响。
- **第二次卖出**时，我们基于第二次买入的价格获得利润，这进一步增加了我们的总利润。

在这个过程中，每一步的决策都依赖于前一步的结果。因此，我们需要分别跟踪第一次买卖和第二次买卖的状态，以确保我们可以在每一步做出最优的决策。

### 任意次数交易的情况

当允许进行任意次数的交易时，问题的性质发生了变化。在这种情况下，我们不再受到交易次数的限制，只需在每个交易日决定是持有股票还是不持有股票。

- **不持有股票状态**：这意味着我们要么刚刚卖出股票，要么处于等待买入的时机。此状态下的最大利润是由之前不持有股票时的利润或通过卖出股票所获得的利润中的较大者确定。
- **持有股票状态**：这意味着我们要么刚刚买入股票，要么持有已买入的股票。此状态下的最大利润是由之前持有股票时的利润或通过新买入股票（减去成本）所获得的利润中的较大者确定。

在任意次数交易的情况下，我们每天的决策更加灵活，不再需要跟踪交易的次序或依赖于前一次交易的结果。我们只需在每一天结束时优化持有或不持有股票的状态即可，这大大简化了问题的复杂度。

总结来说，最多两次交易的情况下需要跟踪每次交易的状态是因为交易之间存在依赖关系，而在任意次交易的情况下，由于不存在交易次数的限制，我们只需优化每天的持有或不持有股票的状态，无需考虑交易的具体次数或顺序。





​		LC188

​		题目链接：[ Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)
​		解题思路；

  1. dp数组

     与上一道题类似，之不过状态不止5个，而是2*k+1个

  2. 递推公式

     j为0，1（第一次持有），2（第一次不持有），3，4，5，6，……，2*k

     ```C++
     dp[i][1] = max(dp[i-1][0]-prices[i],dp[i-1][1]);
     dp[i][2] = max(dp[i-1][1] + prices[i], dp[i-1][2]);
     依次类推
     
     for(int j = 0; j < 2 * k - 1 ; j+=2)
     {
     	dp[i][j+1] = max(dp[i-1][j] - prices[i], dp[i-1][j+1]);
     	dp[i][j+2] = max(dp[i-1][j+1] + prices[i],dp[i-1][j+2]);
     }
     ```
     

3. 初始化

   ```
   dp[0][0] = 0;
   dp[0][1] = -prices[0];
   dp[0][2] = 0;
   
   推出dp[0][j]当j为奇数的时候都初始化为 -prices[0]
   
   for(int j = 1; j < 2 * k; j += 2)
   {
   	dp[0][j] = - prices[0];
   }
   ```

4. 遍历顺序，从前往后

   ```C++
   class Solution {
   public:
       int maxProfit(int k, vector<int>& prices) {
           vector<vector<int>> dp(prices.size(),vector<int>(2*k+1,0));
           
           //初始化
           for(int j = 1; j < 2*k; j+= 2)
           {
               dp[0][j] = - prices[0];
           }
   
           for(int i = 1; i < prices.size(); i++)
           {
              for(int j = 0; j < 2 * k - 1 ; j+=2)
               {
                   dp[i][j+1] = max(dp[i-1][j] - prices[i], dp[i-1][j+1]);
   	            dp[i][j+2] = max(dp[i-1][j+1] + prices[i],dp[i-1][j+2]);
               }
           }
           
           return dp[prices.size()-1][2*k];
       }
   };
   ```

   
