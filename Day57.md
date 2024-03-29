Day 57

​	LC1143

​		题目链接：[Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

​		解题思路：

   			1. dp[i] [j] 表示 以下标i结尾的字符串text1和 以下标为j结尾的字符串text2. 它们的最长公共子序列的长度为dp[i] [j].	

```C+=
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size()+1,vector<int>(text2.size()+1,0));
        int result = 0;

      

        for(int i = 1; i <= text1.size(); i++)
        {
            for(int j = 1; j <= text2.size(); j++)
            {
                if(text1[i-1] == text2[j-1])
                {
                    dp[i][j] = dp[i-1][j-1]+1;
                }
                else
                {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);

                }
                
                
            }
           
        }

        return dp[text1.size()][text2.size()];
        
    }
};
```

​	最长子串（子数组）和最长子序列的区别在于最长子数组要求是连续的，是连在一起的。最长子序列只要保证相对位置相同就行。

​	最长子串：

- **连续性要求**：最长公共子串需要字符连续出现。这意味着，一旦当前字符不匹配，当前考察的公共子串就中断了，需要重新开始计算新的公共子串的长度。
- **全局最大值**：由于子串必须是连续的，所以即使`dp[i][j]`在表的某个点达到了局部最大值，后续的不匹配字符会使得任何新的公共子串长度重新计算。因此，我们需要一个全局变量`result`来记录遍历过程中遇到的最长的公共子串的长度。





​	最长子数组：

- **非连续性**：LCS问题允许子序列在原字符串中是非连续的。这意味着，即使两个字符在当前位置不匹配，它并不影响之前找到的最长公共子序列的有效性。因此，LCS的长度只能在发现匹配的字符时增加，而不匹配的情况下，我们只是尝试找到包含当前字符和不包含当前字符的最长公共子序列中的较大值。

- **动态规划表（`dp`数组）更新逻辑**：在LCS问题中，`dp[i][j]`定义为`text1`的前`i`个字符和`text2`的前`j`个字符的最长公共子序列的长度。对于`dp`表中的任意一点`(i, j)`：

  - 如果`text1[i-1] == text2[j-1]`，则`dp[i][j] = dp[i-1][j-1] + 1`，因为找到了一个属于LCS的新字符。
  - 如果`text1[i-1] != text2[j-1]`，则`dp[i][j] = max(dp[i-1][j], dp[i][j-1])`，表示最长公共子序列可能通过忽略`text1`的第`i`个字符或`text2`的第`j`个字符来获取。

  ### 为什么不需要`result`变量

  由于每个`dp[i][j]`的值都是基于前一个或多个状态计算出来的，且这些值随着算法的进行不断累积，因此`dp`表的最后一个元素`dp[m][n]`（其中`m`和`n`分别为`text1`和`text2`的长度）自然包含了整个问题的解，即`text1`和`text2`的最长公共子序列的长度。换句话说，在LCS问题的动态规划解法中，最终的结果是逐步构建的，最后一次更新的`dp[m][n]`值就是我们要找的最长公共子序列的长度。因此，与最长公共子串问题不同，我们不需要在遍历过程中记录一个全局最大值，因为`dp`数组的最后一个值已经代表了所求的最大长度。

  

​	LC1035

​		题目链接：[Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines/)

​		解题思路：

​			最长公共子序列

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size()+1,vector<int>(nums2.size()+1,0));
        //dp[i][j] 表示 nums1中前i个字符构成的字符串和num2中前j个字符构成的字符串，LCS为dp[i][j]
        for(int i = 1; i <= nums1.size(); i++)
        {
            for(int j = 1; j <= nums2.size(); j++)
            {
                if(nums1[i-1] == nums2[j-1])
                {
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else
                {
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[nums1.size()][nums2.size()];
        
    }
};
```

​	

​	LC53

​	题目链接：[Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

​	解题思路：

  1. dp[i] 表示以nums[i]结尾的数组中，最大的子数组的和为dp[i]

  2. 递推公式,加入nums[i]这个元素到子串中；以及不加入这个元素到前面的子串中，就是重新开始考虑（因为是子数组，也就是子串，所以是要求连续）

     dp[i] = max(dp[i-1]+nums[i],nums[i])

     ```C++
     class Solution {
     public:
         int maxSubArray(vector<int>& nums) {
             vector<int> dp(nums.size());
             dp[0] = nums[0];
             int result = dp[0];
     
             for(int i = 1; i < nums.size(); i++)
             {
                 dp[i] = max(dp[i-1]+nums[i],nums[i]);
                 if(dp[i] > result)
                 {
                     result = dp[i];
                 }
             }
     
             return result;
             
             
         }
     };
     ```

     
