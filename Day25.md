Day 25 LC216,LC17



​	LC216

​		题目链接：[ Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

​		解题思路：

​		 	k是树的深度，n是树的宽度。

​			剪枝操作：

   1. for循环的地方，可以根据元素的剩余个数以及还需要的元素个数，进行剪枝。还需要的元素个数为k-path.size(),剩余的元素个数为9-i+1。要求9-i+1 >= k - path.size()

   2. 已选元素总和如果已经大于n（图中数值为4）了，那么往后遍历就没有意义了，直接剪掉。这个剪枝步骤放在递归函数最前面比较合理。

      ```C++
      class Solution {
      private:
          vector<vector<int>> result;
          vector<int> path;
          void backtracking(int k,int n,int startItem,int curSum)
          {
              if(curSum > n)
              {
                  return;
              }
              if(path.size() == k)
              {
                  if(curSum == n)
                  {
                      result.push_back(path);
                      return;
                  }
                  else
                  {
                      return;
                  }
              }
              for(int i = startItem; i <= path.size()- k + 10; i++)
              {
                  curSum += i;
                  path.push_back(i);
                  backtracking(k,n,i+1,curSum);
                  curSum -= i;
                  path.pop_back();
              }
              
          }
      public:
          vector<vector<int>> combinationSum3(int k, int n) {
              backtracking(k,n,1,0);
              return result;
      
              
          }
      };
      ```

      





​	LC17

​		题目链接：[Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)



​		解题思路：

   1. 本题仍然是使用回溯来完成。首先需要设置一个index，来遍历输入参数string.只有这样才能找到string中每个数字字符对应的Letters是什么。所以回溯模板中的for循环遍历的是找到的letters。递归的深度是有index决定的，因为它决定了什么时候收集结果，也决定了什么时候遍历完string。但是也可以根据path.size()的长度来做终止条件。

      一次AC

​	
