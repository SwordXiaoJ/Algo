Day 51

​		LC139

​		题目链接： [Word Break](https://leetcode.com/problems/word-break/)

​		解题思路：

   1. DP问题

      背包是单词，物品是wordDict，看看物品能不能把背包装满。因为wordDict可以反复利用，所以是一个完全背包。

      而本题其实我们求的是排列数，为什么呢。 拿 s = "applepenapple", wordDict = ["apple", "pen"] 举例。

      "apple", "pen" 是物品，那么我们要求 物品的组合一定是 "apple" + "pen" + "apple" 才能组成 "applepenapple"。

      "apple" + "apple" + "pen" 或者 "pen" + "apple" + "apple" 是不可以的，那么我们就是强调物品之间顺序。

      ```C++
      class Solution {
      public:
          bool wordBreak(string s, vector<string>& wordDict) {
              //dp[j]表示长度为j的字符串，是否可以由wordDict中的元素组成，为true或者false。背包为字符串s,单词是物品。
      
              vector<bool> dp(s.size()+1,false);
              unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
      
              //递推公式
              //每一个背包是否可以装满，取决于背包j（长度）所表示的位置，把字符串分为两部分。如果后一部分可以在wordDict中找到，前一部分为true，那么就为true
      
              //初始化
              dp[0] = true;
      
              //本题需要先遍历背包，在遍历物品。因为我们需要顺序，所以是排列
              for(int j = 1; j <= s.size(); j++)
              {
                  for(int i = 0; i < j; i++)
                  {
                      string secondPart = s.substr(i,j-i);
               
                      if (wordSet.find(secondPart) != wordSet.end() && dp[i]) 
                      {
                          dp[j] = true;
                      }
      
                  }
              }
              return dp[s.size()];
      
              
          }
      };
      ```

      

