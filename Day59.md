Day 59

​	LC583

​		题目链接：[Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)

​		解题思路：

   1. dp[i] [j] 表示长度为i的字符串word1和长度为j的字符串word2,要达到相同，需要的最少步数为dp[i] [j]

   2. 

      ```c++
      if(word1[i-1] == word2[j-1]) // 如果相同,去看之前的要多少步
      {
          dp[i][j] = dp[i-1][j-1];
      }
      else //如果不相同
      {
          //case1: 删除word1[i-1] dp[i-1][j] + 1
          //case2: 删除word2[j-1] dp[i][j-1] + 1
          //case3: 都删除，dp[i-1][j-1] + 2
          dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1] + 2);
          
          
      }
      ```

      

   3.  初始化

         ```C++
         class Solution {
         public:
             int minDistance(string word1, string word2) {
                 vector<vector<int>> dp(word1.size()+1,vector<int>(word2.size()+1,0));
         
                 for(int i = 0; i <= word1.size(); i++)
                 {
                     dp[i][0] = i;
                 }
                 for(int j = 0; j <= word2.size(); j++)
                 {
                     dp[0][j] = j;
                 }
         
         
                 for(int i = 1; i <= word1.size(); i++)
                 {
                     for(int j = 1; j <= word2.size(); j++)
                     {
                         if(word1[i-1] == word2[j-1])
                         {
                             dp[i][j] = dp[i-1][j-1];
                         }
                         else
                         {
                             dp[i][j] = min(dp[i-1][j] + 1, min(dp[i][j-1] + 1, dp[i-1][j-1] + 2));
                         }
         
                     }
                 }
         
                 return dp[word1.size()][word2.size()];
                 
             }
         };
         ```

          

​	LC72



​		题目链接：[Edit Distance](https://leetcode.com/problems/edit-distance/)

​		解题思路：

   1. dp[i] [j] 表示长度为i 的word1和长度为j 的 word2， 最小的编辑距离为dp[i] [j]

   2.  递推公式

      ```
      if(word1[i-1] == word2[j-1]) // 如果相同,去看之前的要多少步
      {
          dp[i][j] = dp[i-1][j-1];
      }
      else //如果不相同
      {
          //case1:word1 这个元素删除， dp[i-1][j] + 1
          //case2: word1 在word1[i-1]后面增加一个元素和word2[j-1]相同。 就是dp[i][j-1] + 1
          //case3: word1[i-1]这个元素进行替换，使它和word2[j-1]相同。那么就是需要看之前的的要多少步。就是dp[i-1][j-1] + 1
          dp[i][j] = min( dp[i-1][j] + 1,dp[i][j-1] + 1,dp[i-1][j-1] + 1);
          
      }
      ```

      

   3. x

      ```C++ 
      class Solution {
      public:
          int minDistance(string word1, string word2) {
              vector<vector<int>> dp(word1.size()+1,vector<int>(word2.size()+1,0));
              for(int i = 0; i <= word1.size(); i++)
              {
                  dp[i][0] = i;
              }
              for(int j = 0; j <= word2.size(); j++)
              {
                  dp[0][j] = j;
              }
      
              for(int i = 1; i <= word1.size(); i++)
              {
                  for(int j = 1; j <= word2.size(); j++)
                  {
                      if(word1[i-1] == word2[j-1])
                      {
                          dp[i][j] = dp[i-1][j-1];
                      }
                      else
                      {
                          dp[i][j] = min(min(dp[i-1][j]+1,dp[i][j-1]+1),dp[i-1][j-1]+1);
                      }
                  }
              }
      
              return dp[word1.size()][word2.size()];
              
          }
      };
      ```

      



​		
