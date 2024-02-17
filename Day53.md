Day 53

​		LC121

​		题目链接：

​		[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

​		解题思路：

   1.   贪心（一次遍历）

      
      
      ```C++
      class Solution {
      public:
          int maxProfit(vector<int>& prices) {
              int low = INT_MAX;
              int result = 0;
              for(int i = 0; i < prices.size(); i++)
              {
                  low = min(low,prices[i]);
                  result = max(result,prices[i] - low);
              }
              return result;
           
          }
	};
      ```

	
	
   1.   DP

      ```C++
      
      ```
      
      1. 确定DP数组含义
      
         dp[i] [0] 表示第i天**持有**股票所得的最多现金
      
         dp[i] [1] 表示第i天**不持有**股票所得的最多现金
      
         **注意这里说的是“持有”，“持有”不代表就是当天“买入”！也有可能是昨天就买入了，今天保持持有的状态**  不持有，不完全代表当天卖出。可以是当天卖出，也可以是之前卖出
      
         
      
         其实一开始现金是0，那么加入第i天买入股票现金就是 -prices[i]， 这是一个负数。
      
	   这里不使用买入，卖出作为状态，是因为这样会复杂很多。如果将买入，卖出作为状态，那么还需要定义其他的状态，保持买入状态，保持卖出状态等，比较复杂。 使用持有来定义，已经包含了买入和保持， 卖出和保持。
      
	   
	
	2.  递归公式
	
	    如果第i天持有股票即dp[i] [0]， 那么可以由两个状态推出来。
	
	   - 第i-1天就持有股票，那么就保持现状，所得现金就是昨天持有股票的所得现金 即：dp[i - 1] [0]
	
	   - 第i天买入股票，所得现金就是买入今天的股票后所得现金即：-prices[i]。 
	
	     **为什么不是dp[i-1] [1] - prices[i],是因为只能允许买一次，第i天买入这个股票，那么之前应该是不能买/卖，所以利润一直是0。**
	
	   ```C++
	   dp[i][0] = max(dp[i-1][0],-prices[i]);
	   ```
	
	   
	
	   如果第i天不持有股票，即dp[i] [1],可以由以下两个状态推导出来
	
	   - 第i-1天就不持有股票，那么就保持现状，所得现金就是昨天不持有股票的所得现金 即：dp[i - 1] [1]          i-1天不持有股票，可能是之前已经进行过一次买入/卖出操作，也可以是从头到尾没有买/卖操作。
	   - 第i天卖出股票，所得现金就是按照今天股票价格卖出后所得现金即：prices[i] + dp[i - 1] [0]
	
	   ```
	   dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1] [0]);
	   ```
	
	   
	
	```C++
	class Solution {
	public:
	    int maxProfit(vector<int>& prices) {
	
	        //dp数组定义
	        //dp[i] [0] 表示第i天**持有**股票所得的最多现金
	        //dp[i] [1] 表示第i天**不持有**股票所得的最多现金
	
	        vector<vector<int>> dp(prices.size(),vector<int>(2,0));
	        
	        //递推公式
	        //如果第i天持有股票即dp[i] [0]， 那么可以由两个状态推出来。第i-1天就持有股票，那么就保持现状，所得现金就是昨天持有股票的所得现金 即：dp[i - 1] [0]。第i天买入股票，所得现金就是买入今天的股票后所得现金即：-prices[i]。
	        //如果第i天不持有股票，即dp[i] [1],可以由以下两个状态推导出来- 第i-1天就不持有股票，那么就保持现状，所得现金就是昨天不持有股票的所得现金 即：dp[i - 1] [1] i-1天不持有股票，可能是之前已经进行过一次买入/卖出操作，也可以是从头到尾没有买/卖操作- 第i天卖出股票，所得现金就是按照今天股票价格卖出后所得现金即：prices[i] + dp[i - 1] [0]
	
	 
	        //dp[i][0] = max(dp[i-1][0],-prices[i]);
	        //dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1] [0]);
	
	
	        //初始化
	        dp[0][0] = -prices[0];
	        dp[0][1] = 0;
	
	        //遍历顺序，从递推公式可以看出，从前往后
	        for(int i = 1; i < prices.size(); i++)
	        {
	            dp[i][0] = max(dp[i-1][0],-prices[i]);
	            dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1] [0]);
	        }
	        return max(dp[prices.size()-1][1],dp[prices.size()-1][0]);    
	    }
	};
	```
	
	

​		LC122

​		题目链接：

​		[Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

​		解题思路：

​			基本与LC121一致，注意因为可以多次买卖，所以在计算dp[i] [0]的时候，有所变化。第一种情况是第i-1天就持有股票，这个不变。第二种情况是第i天买入股票，此时因为是可以多次买卖，所以是dp[i-1] [1] - prices[i] ， 而不再是 - prices[i].  因为多次买卖，所持有的现金可能有之前买卖过的利润，而不是0.

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        //dp数组定义
        //dp[i][0] 表示第i天持有股票所得的最多现金
        //dp[i][1] 表示第i天不持有股票所得的最多现金
        vector<vector<int>> dp(prices.size(), vector<int>(2, 0));


        //递推公式
        //如果第i天持有股票即dp[i][0]， 那么可以由两个状态推出来
        //第i-1天就持有股票，那么就保持现状，所得现金就是昨天持有股票的所得现金 即：dp[i - 1][0]
        //第i天买入股票，所得现金就是昨天不持有股票的所得现金减去 今天的股票价格 即：dp[i - 1][1] - prices[i]
        //dp[i][0] = max(dp[i-1][0],dp[i-1][1] - prices[i])


        //初始化
        dp[0][0] -= prices[0];
        dp[0][1] = 0;


        for(int i = 1; i < prices.size(); i++)
        {
            dp[i][0] = max(dp[i-1][0],dp[i-1][1] - prices[i]);
            dp[i][1] = max(dp[i-1][1],dp[i-1][0] + prices[i]);

        }
        return max(dp[prices.size()-1][0], dp[prices.size()-1][1]);
    }
};
```

