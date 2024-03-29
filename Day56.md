Day 56

​	LC300

​		题目链接：[Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

​		解题思路：

   1. dp数组定义

        dp[i] 表示以number[i] 为结尾的最长递增子序列的长度。

      为什么一定表示 “以nums[i]结尾的最长递增子序” ，因为我们在 做 递增比较的时候，如果比较 nums[j] 和 nums[i] 的大小，那么两个递增子序列一定分别以nums[j]为结尾 和 nums[i]为结尾， 要不然这个比较就没有意义了，不是尾部元素的比较那么 如何算递增呢。

      

   2. 递推公式

      j是从0到i-1

      if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);

      直接设置 `dp[i] = dp[j] + 1` 可能会忽略一个重要的细节：对于每个 `i`，可能存在多个不同的 `j` 满足 `nums[j] < nums[i]`，而这些不同的 `j` 可能会导致不同长度的递增子序列。

      为了确保 `dp[i]` 真正反映以 `nums[i]` 结尾的最长递增子序列的长度，我们需要在所有可能的 `j` 中找到那个能够给出最长子序列长度的 `j`。这就是为什么我们更新 `dp[i]` 时要使用 `max(dp[i], dp[j] + 1)` 而不是直接赋值为 `dp[j] + 1`。

   3. 初始化

      dp[i] 起始大小为1，因为至少包含一个数字。

   4. dp[i] 依赖于前面的数字进行比较，所以i是从前往后。j都可以，所以从前往后就可以。

   5. 注意最后的返回值并不是dp[num.size() - 1]，因为最长递增子序列并不一定包含最后一个元素，并不一定是以最后一个元素结尾的最长递增子序列的长度。所以需要一个result去记录结果。不断更新。

      ```C++
      class Solution {
      public:
          int lengthOfLIS(vector<int>& nums) {
              if(nums.size() == 1) return nums.size();
              vector<int> dp(nums.size(),1);
              int result = 0;
              for(int i = 1; i < nums.size(); i++)
              {
                  for(int j = 0; j < i; j++)
                  {
                      if(nums[i] > nums[j])
                      {
                          dp[i] = max(dp[i],dp[j] + 1);
                      }
                  }
                  if(dp[i] > result) result = dp[i];
              }
              return result;
      
              
          }
      };
      ```

      

​	LC674

​		题目链接：[Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)

​		解题思路：

   1. **dp[i]：以下标i为结尾的连续递增的子序列长度为dp[i]**。

   2.  if(dp[i] > dp[i-1]) dp[i] = dp[i-1] + 1;

      因为本题要求连续递增子序列，所以就只要比较nums[i]与nums[i - 1]，而不用去比较nums[j]与nums[i] （j是在0到i之间遍历）。

      既然不用j了，那么也不用两层for循环，本题一层for循环就行，比较nums[i] 和 nums[i - 1]。

      ```C++
      class Solution {
      public:
          int findLengthOfLCIS(vector<int>& nums) {
              vector<int> dp(nums.size(),1);
              if(nums.size() == 1) return 1;
              int result = 1;
      
              for(int i = 1; i < nums.size(); i++)
              {
                  if(nums[i] > nums[i-1])
                  {
                      dp[i] = dp[i-1] + 1;
                  }
                  if(dp[i] > result)
                  {
                      result = dp[i];
                  }
      
              }
              return result;
              
          }
      };
      ```

      

​	LC718

​		题目链接：[Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

​		解题思路：

​		子数组其实就是连续子序列

   1. 递归数组定义

      ​	dp[i] [j] 表示从以下标i结尾的A和以下标j结尾的B，它们的最长重复子数组的长度为dp[i] [j] 

   2. 递推公式

         如果A[i-1] == B[j-1]

         dp[i] [j] = dp[i-1] [j-1] + 1

   3. 初始化

         

   4. 循环顺序

         外层循环A，内层循环B。或者返回过也可

         ```C++
         class Solution {
         public:
             int findLength(vector<int>& nums1, vector<int>& nums2) {
                 vector<vector<int>> dp(nums1.size()+1,vector<int>(nums2.size()+1,0));
                 int result = 0;
         
                 for(int i = 0; i < nums1.size(); i++)
                 {
                     if(nums1[i] == nums2[0])
                     {
                         dp[i][0] = 1;
                     }
                     if(dp[i][0] > result)
                     {
                         result = dp[i][0];
                     }
                 }
                 for(int j = 0; j < nums2.size(); j++)
                 {
                     if(nums1[0] == nums2[j])
                     {
                         dp[0][j] = 1;
                     }
                     if(dp[0][j] > result)
                     {
                         result = dp[0][j];
                     }
                     
                 }
         
         
         
                 for(int i = 1; i < nums1.size(); i++)
                 {
                     for(int j = 1; j < nums2.size(); j++)
                     {
                         if(nums1[i] == nums2[j])
                         {
                             dp[i][j] = dp[i-1][j-1] + 1;
                         }
                         if(dp[i][j] > result)
                         {
                             result = dp[i][j];
                         }
                     }
                 }
         
                 return result;
                 
             }
         };
         ```

         


​	我采用的是最直接的定义方法，所以会在初始化的部分有点复杂。如果想要代码简化，可以定义dp[i] [j] ：以下标i - 1为结尾的A，和以下标j - 1为结尾的B，最长重复子数组长度为dp[i] [j]。 （**特别注意**： “以下标i - 1为结尾的A” 标明一定是 以A[i-1]为结尾的字符串 ）

​	

```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp (nums1.size() + 1, vector<int>(nums2.size() + 1, 0));
        int result = 0;
        for (int i = 1; i <= nums1.size(); i++) {
            for (int j = 1; j <= nums2.size(); j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }
                if (dp[i][j] > result) result = dp[i][j];
            }
        }
        return result;
    }
};
```

