Day 34

​	LC1005

​	题目链接：[Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)

​	解题思路：

​		一次AC

​		局部贪心：让绝对值最大的负数变为正数，当全部为正数的时候，只翻转数值最小的正数。

​		所以先使用绝对值大小从大到小对数组进行排序，然后遍历数组，当遇到负数的时候，进行翻转，并且k自减。做完所有的负数翻转k还没变为0，再对最后一个元素进行反复翻转，直到用完k。





​	LC134

​	题目链接：[Gas Station](https://leetcode.com/problems/gas-station/)

​	解题思路：

​	方法一一次AC

  1. for循环，统计rest数组，rest数组，就是每个加油站剩余的油量。走一圈就是把这些剩余油量加起来。从头开始统计累加，如果累加的值小于0，说明从[0,i]这个区间的任何一个点出发，都不能走一圈。因为根本过不了这个i这个加油站，因为及时前面都是累加正数，到i这个站点也变为了负数。所以只能从i+1出发，重新开始统计。

     ```C++
     class Solution {
     public:
         int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
             vector<int> rest(gas.size());
             int curSum = 0;
             int result = 0;
             int totalSum = 0;
             for(int i = 0; i < gas.size(); i++)
             {
                 rest[i] = gas[i] - cost[i];
                 curSum += rest[i];
                 totalSum += rest[i];
                 if(curSum < 0)
                 {
                     curSum = 0;
                     result = i + 1;
                 }
             }
     
             if(totalSum < 0)
             {
                 return -1;
             }
             return result;
             
             
         }
     };
     ```

     

  2. 暴力解法（会超时）

     ```C++
     class Solution {
     public:
         int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
             for(int i = 0; i < gas.size(); i++)
             {
                 int rest = gas[i] - cost[i];
                 if(rest < 0)
                 {
                     continue;
                 }
                 int start = i;
                 start = (start+1) % gas.size();
                
                 while(start != i)
                 {
                     rest +=  (gas[start] - cost[start]);
                   
                     if(rest < 0)
                     {
                         break;
                     }
                     start = (start+1) % gas.size();
                 }
     
                 if(rest >= 0 && start == i)
                 {
                     return i;
                 }
             }
             return -1;
     
            
             
         }
     };
     ```

     



​	LC135

​	题目链接：[Candy](https://leetcode.com/problems/candy/)

​	解题思路：

  1. 首先从左到右遍历，此时局部最优：只要右边评分比左边大，右边的孩子就多一个糖果，全局最优：相邻的孩子中，评分高的右孩子获得比左边孩子更多的糖果

  2. 然后从右向左遍历（从后向前），处理左孩子大于右孩子的情况。

     为什么不能从前向后遍历呢？因为 rating[5]与rating[4]的比较 要利用上 rating[5]与rating[6]的比较结果，所以 要从后向前遍历。

     如果 ratings[i] > ratings[i + 1]，此时candys[i]（第i个小孩的糖果数量）就有两个选择了，一个是candys[i + 1] + 1（从右边这个加1得到的糖果数量），一个是candys[i]（之前比较右孩子大于左孩子得到的糖果数量）。

     那么又要贪心了，局部最优：取candys[i + 1] + 1 和 candys[i] 最大的糖果数量，保证第i个小孩的糖果数量既大于左边的也大于右边的。全局最优：相邻的孩子中，评分高的孩子获得更多的糖果。有点像动态规划

     ```C++
     class Solution {
     public:
         int candy(vector<int>& ratings) {
             vector<int> candys(ratings.size(),1);
             for(int i = 0; i < ratings.size() - 1; i++)
             {
                 if(ratings[i+1] > ratings[i])
                 {
                     candys[i+1] = candys[i] + 1;
                 }
             }
     
             for(int j = ratings.size() - 1; j >= 1; j--)
             {
                 if(ratings[j-1] > ratings[j])
                 {
                     candys[j-1] = max(candys[j-1], candys[j] + 1);
                 }
     
             }
             // 统计结果
             int result = 0;
             for (int i = 0; i < candys.size(); i++) result += candys[i];
             return result;
             
         }
     };
     ```

     

