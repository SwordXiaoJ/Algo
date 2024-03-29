Day 58

​	LC392

​		题目链接：[Is Subsequence](https://leetcode.com/problems/is-subsequence/)

​		解题思路：

1. dp[i] [j] 表示长度为i的字符串a,和长度为j的字符串b，它们的相同子序列的长度为为dp[i] [j]。它们的结尾的

2. 递推公式

   其实就是当前的字符不匹配了的话，那么就只能取之前匹配的结果。但是因为只能移动t的下标而已

   ```C++ 
   if(s[i-1] == t[j-1]) //找到相同的字符
   {
       dp[i][j] = dp[i-1][j-1] + 1;
   }
   else //没有找到相同的字符，此时t就需要删除一个字符。t如果把当前元素t[j - 1]删除，那么dp[i][j] 的数值就是 看s[i - 1]与 t[j - 2]的比较结果了，即：dp[i][j] = dp[i][j - 1]; 其实就是当前的字符不匹配了的话，那么就只能取之前匹配的结果。但是因为只能移动t的下标而已
   {
       dp[i][j] = dp[i][j-1]
   }
   ```

3. 从递推公式可以看出dp[i ][j]都是依赖于dp[i - 1] [j - 1] 和 dp[i] [j - 1]，所以dp[0] [0]和dp[i] [0]是一定要初始化的。

   这里大家已经可以发现，在定义dp[i] [j]含义的时候为什么要**表示以下标i-1为结尾的字符串s，和以下标j-1为结尾的字符串t，相同子序列的长度为dp[i] [j]**。

4. ```C++
   class Solution {
   public:
       bool isSubsequence(string s, string t) {
           vector<vector<int>> dp(s.size()+1, vector<int>(t.size()+1,0));
   
   
           for(int i = 1; i <= s.size(); i++)
           {
               for(int j = 1; j <= t.size(); j++)
               {
                   if(s[i-1] == t[j-1])
                   {
                       dp[i][j] = dp[i-1][j-1] + 1;
                   }
                   else
                   {
                       dp[i][j] = dp[i][j-1];
                   }
               }
           }
   
           if(dp[s.size()][t.size()] == s.size())
           {
               return true;
           }
           return false;
   
           
       }
   };
   ```

   注意和最长公共子序列的区别是 最长公共子序列在不匹配的情况下，可以移动两个字符串的下标。这个里面，只能移动t的下标。

5.  Notes： 此类题的dp定义是长度而不是下标，所以遍历的时候，需要遍历到s.size()









​	LC115

​		题目链接：[Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

​		解题思路：

   1. dp[i] [j] 表示长度为i的字符串s和长度为j的字符串t, s字符串包含t的数量为dp[i] [j]

   2. 如果s[i-1] == t[j-1], 这个位置两个元素已经相同了。所以要不就是这个s[i-1]这个元素被归入使用，要不就是不使用它（在之前的字符串中去寻找匹配）

      1. 使用s[i-1]，那么就需要同时使用s[i-1] 和 t[j-1]这两个元素，它们既然相等，所以只需要考虑s[i-2]和t[j-2]两个位置元素的dp情况。

      2. 不去使用s[i-1]这个元素，模拟其实就是把这个s[i-1]删除掉。所以其实考虑的就是s[i-2]和t[j-1]的匹配情况

         例如： s：bagg 和 t：bag ，s[3] 和 t[2]是相同的，但是字符串s也可以不用s[3]来匹配，即用s[0]s[1]s[2]组成的bag。

         当然也可以用s[3]来匹配，即：s[0]s[1]s[3]组成的bag。

   3. 如果s[i-1] != t[j-1]，那么很简单，那么就需要去匹配s[i-2]和t[j-1]的结果。就是不去使用s[i-1]，也就是删除s[i-1]

      ```C++ 
      if(s[i-1] == t[j-1])
      {
          dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
      }
      else
      {
          dp[i][j] = dp[i-1][j]
      }
      ```



4. 初始化

   从递归公式可以看出，是从左上和上方来的，所以dp[0] [j] 和 dp[i] [0] 要初始化。

   dp[0] [j]  表示 s字符串长度为0，其中出现t字符串的数量，都为0

   dp[i] [0] 表示 s字符串长度为i, 其中出现空字符串t的数量，都为1。因为可以删除所有元素，使它为空字符串

   dp[0] [0] 应该为1.

   ```C++
   class Solution {
   public:
       int numDistinct(string s, string t) {
           vector<vector<u_int>> dp(s.size()+1,vector<u_int>(t.size()+1,0));
   
           for(int i = 0; i <= s.size(); i++)
           {
               dp[i][0] = 1;
           }
           for(int i = 1; i <= s.size(); i++)
           {
               for(int j = 1; j <= t.size(); j++)
               {
                   if(s[i-1] == t[j-1])
                   {
                       dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
                   }
                   else
                   {
                       dp[i][j] = dp[i-1][j];
                   }
               }
           }
           return dp[s.size()][t.size()];
           
       }
   };
   ```

   

​		
