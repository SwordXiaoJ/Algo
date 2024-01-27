Day 32



LC122 

​	题目链接：[Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

​	解题思路：
​		贪心的想法就是挑一个低的买入，高的卖出，循环往复。

​		但是注意利润可以分解，prices[3] - prices[0] = prices[3] - prices[2] + prices[2] - prices[1] + prices[1] - prices[0].

​		也就是利润差之和。所以根据原来的数组，可以得到每天的利润数组。然后挑出来利润数组中的正数就可以。

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        
        for(int i = 1; i < prices.size(); i++)
        {
            if(prices[i] - prices[i-1] > 0)
            {
                result += (prices[i] - prices[i-1]);
            }

        }
        return result;
        
    }
};
```



LC55

​	题目链接：https://leetcode.com/problems/jump-game/

​	解题思路：

​		贪心策略：每次走最大的步数，看看最后能不能覆盖最后一个元素。在这个范围内，不管怎么跳，一定可跳过来的。

​		但是实际写代码的时候，是要在这个最大的cover范围内遍历，去确定这个范围内会不会有新的cover值。

​		而不是每次都跳最大步，然后根据最大步达到的的位置，更新cover.这样会漏掉一些情况，例如3,0,2,0,1

​		

```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {

        int cover = nums[0];
        for(int i = 0; i <= cover; i++)
        {
            cover = max(cover,i+nums[i]);
            if(cover >= nums.size()-1)
            {
                return true;
            }

        }
        return false;
        
    }
};
```



LC45

​	题目链接：[Jump Game II](https://leetcode.com/problems/jump-game-ii/)

​	解题思路：

​		贪心思路： 每一步尽可能多走，如果还没到达终点，就步数加一。但是在代码中，不可以每次都跳最大值。

  1. 遍历每个点，更新可达范围。但是注意的是，每次遍历都是在可达范围内遍历。如果遍历到其中一个点，更新后的可达范围到达了终点，就跳出循环。如果遍历完当前的可达范围，都不能到达终点，统计的步数加一。

  2. 一个可达范围内的点，都是跳一次就可以到。所以就是如果这个范围内到不了，就加一步。去试探新的可达范围。

     ```C++
     class Solution {
     public:
         int jump(vector<int>& nums) {
             int curCover = nums[0];
             if(nums.size() == 1) return 0;
             int count = 1;
             int nextCover = 0;
             for(int i = 1; i < nums.size(); i++)
             {
                 nextCover = max(nextCover,i+nums[i]);
                 if(curCover >= nums.size() - 1) break;
                 if(i == curCover)
                 {
                     count++;
                     curCover = nextCover;
                 } 
                 
             }
             return count;
             
         }
     };
     ```


     和题解不同的是，我是利用curCover进行判断，break在if外面。

     
