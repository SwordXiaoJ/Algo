Day 43

​	题目链接：[Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

​	解题思路：

​		选石头碰撞的过程抽象成其实时把一个数组里面各个元素加或者减的过程，即给每一个元素乘1或者乘-1。要求希望最后剩余的值最小。那么其实可以整理表达式，把正数放在一堆，负数放在一堆。最后做一个减法即可。

​		因此题目可以转换为将数组中的元素尽可能分为相等的两组。

​		那么分成两堆石头，一堆石头的总重量是dp[target]，另一堆就是sum - dp[target]。

**在计算target的时候，target = sum / 2 因为是向下取整，所以sum - dp[target] 一定是大于等于dp[target]的**。

​	

```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {

        int sum = 0;
        for(int i = 0; i < stones.size(); i++)
        {
            sum += stones[i];
        }
        int target = sum / 2;

        //dp数组的定义
        //dp[j]表示容量为j的背包，可以装得下的最大重量（不一定可以装满，因为输入元素的不同）
        vector<int> dp(target+1,0);

        //递推公式
        //dp[j] = max(dp[j], dp[j-nums[i]] + nums[i])

    
        for(int i = 0; i < stones.size(); i++)
        {
            for(int j = target; j >= stones[i]; j--)
            {
                dp[j] = max(dp[j], dp[j- stones[i]] + stones[i]);
            }
        }

        return sum - dp[target] - dp[target];



        
    }
};
```



LC494

​	题目链接：[Target Sum](https://leetcode.com/problems/target-sum/)

​	解题思路：
  1. 思路一：递归。但是因为有+/-两种选择，所以不能使用传统的递归求和的方式。稍微加以修改。

     ```C++
     class Solution {
     private:
         int count = 0;
         void backtracking(vector<int>& nums,int target,int curSum,int startIndex)
         {
             if(startIndex == nums.size())
             {
                 if(curSum == target)
                 {
                     count++;
                 }
             }
             else
             {
                 backtracking(nums,target,curSum+nums[startIndex],startIndex+1);
                 backtracking(nums,target,curSum-nums[startIndex],startIndex+1);
     
             }
         }
     public:
         int findTargetSumWays(vector<int>& nums, int target) {
             backtracking(nums,target,0,0);
             return count;
             
         }
     };
     ```

     

  2.  类似分石头那道题，其实最终就是left - right = target. 又因为right = sum - left.

     所以就得到2 * left - sum = target. 

     所以 left = (sum + target) / 2;

     其实就是求满足条件的left有多少个的问题。也就是装满这个容量为left的背包，有多少种方法

     

     注意的问题是dp[0] = 1;如果是0，后面所有结果都是0.

     也符合实际意义，如果Nums=[0],target = 0,那么dp[0] = 1;

     

     两个return 为 0的情况，是test的时候发现的。

     ```C++
     class Solution {
     public:
         int findTargetSumWays(vector<int>& nums, int target) {
             int sum = 0;
             for(int i = 0; i < nums.size(); i++)
             {
                 sum += nums[i];
             }
             if((sum + target) % 2 == 1) return 0;
             if(abs(sum) < abs(target)) return 0;
     
     
             int left = (sum + target) / 2;
             vector<int> dp(left+1,0);
             //dp[j]表示装满容量为j的背包，有dp[j]种方法
     
             //推导公式,如果选择不包含这个元素，那么dp[j]不变，如果选择包含这个元素，则为dp[j-nums[i]]
             //dp[j] = dp[j] + dp[j-nums[i]];
             
             // 这个公式意味着，到达和 i 的总方法数是“不包含当前数字时到达和 i 的方法数”与“包含当前数字时到达和 i-num 的方法数”的总和。
             dp[0] = 1;
             for(int i = 0; i < nums.size(); i++)
             {
                 for(int j = left; j >= nums[i]; j--)
                 {
                     dp[j] = dp[j] + dp[j-nums[i]];
     
                 }
     
             }
     
             return dp[left];
             
             
         }
     };
     ```

     

总结：

 1. 寻找和为特定值的子集数量，这是一个典型的动态规划问题。

 2. 组合问题仅仅是求个数的话，就可以用dp，但如果要求的是把所有组合列出来，还是要使用回溯法暴力搜索的。

    



LC474

​	题目链接：[Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)

​	解题思路：
​		仍然是背包问题，只不过这个背包有两个维度。

```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        //dp[i][j] 表示往 最多有m个0，n个1的背包中装东西。dp[i][j]种装法。
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));


        //转移方程，因为要求的是最大值，所以使用max进行筛选，如果不选这个字符串，就还是dp[i][j],选了就是后者。二者选出最大的
        //dp[i][j] = max(dp[i][j],dp[i-oneNum][j-zeroNum] + 1)

        //初始化
        //都初始化为0就可

        for(string item : strs)
        {
            int oneNum = 0;
            int zeroNum = 0;

            for(char c : item)
            {
                if(c == '1')
                {
                    oneNum++;
                }
                else
                {
                    zeroNum++;
                }
            }
            //遍历背包（从后往前）
            for(int i = m; i >= zeroNum; i--)
            {
                for(int j = n; j >= oneNum; j--)
                {
                    dp[i][j] = max(dp[i][j],dp[i-zeroNum][j-oneNum] + 1);
                }

            }

        }
        return dp[m][n];
    }
};
```







总结：

​	这两个问题虽然都可以用动态规划（DP）解决，但它们分别解决的是不同类型的问题，导致状态转移方程中使用了不同的操作（`max()` vs. `+=`）。

### 第一个问题：找出最大的形式

这个问题是一个典型的0-1背包问题变体，其中你需要在给定的二维（`m`, `n`）限制下，从字符串数组中选出最多的字符串，使得所选字符串中包含的0的数量不超过`m`，且1的数量不超过`n`。这里的状态转移方程使用`max()`是因为你需要在**不选当前字符串**和**选当前字符串**之间做出选择，哪种选择可以使得所选字符串数量最多。这是一个典型的优化问题，你需要找出最优解（即最大的字符串数量）。

### 第二个问题：目标和

这个问题是一个找出所有可能的组合使得它们的和等于一个特定目标值的问题。这里的动态规划数组`dp[j]`表示达到和为`j`的方法总数。对于每个数字`nums[i]`，你有两种选择：使用它或不使用它。这个公式意味着，到达和 i 的总方法数是“不包含当前数字时到达和 i 的方法数”与“包含当前数字时到达和 i-num 的方法数”的总和。这里使用`+=`是因为你在统计所有可能达到特定和的方法数量，这是一个计数问题。
