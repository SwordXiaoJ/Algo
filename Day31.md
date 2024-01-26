Day 31



LC455

​	题目链接：[Assign Cookies](https://leetcode.com/problems/assign-cookies/)

​	解题思路：

  1. 大饼干满足大胃口

     先遍历胃口。

     如果最外层先遍历的是饼干，由于此时先尝试的是最大的胃口，可能最大的饼干也没有满足最大的胃口，此时会一直循环，直到尝试完所有的饼干，返回0.

     ```C++
     class Solution {
     public:
         int findContentChildren(vector<int>& g, vector<int>& s) {
             sort(g.begin(),g.end());
             sort(s.begin(),s.end());
             int result = 0;
             //大饼干给大胃口，最外层循环是胃口。如果最外层是饼干，那么最大的饼干可能没办法满足最大胃口，就会一直循环去更小的饼干，直到返回0.但是最大的饼干可能可以满足次大的胃口。
             int sIndex = s.size()-1;
             for(int i = g.size()-1; i >= 0; i--)
             {
                 if(sIndex >= 0 && s[sIndex] >= g[i])
                 {
                     result++;
                     sIndex--;
                 }
     
             }
             return result;
         }
     };
     ```

     

  2. 小饼干满足小胃口

     先遍历饼干

     如果最外层遍历的是胃口，由于此时尝试的是最小的饼干，可能最出现最小的饼干不能满足最小的胃口，此时胃口会持续循环，越变越大，饼干会一直卡在最小的饼干。最后返回0。

     例如饼干为5，6，7，8    胃口为7，8，9，10

     ```C++
     class Solution {
     public:
         int findContentChildren(vector<int>& g, vector<int>& s) {
             sort(g.begin(),g.end());
             sort(s.begin(),s.end());
             int result = 0;
             //小饼干先满足小胃口。此时最外层循环是饼干。如果最外层是胃口，此时最小的饼干可能会没办法满足最小的胃口，会去尝试更大的胃口。最后返回0.但是可能比小饼干大一些的饼干，可以满足小的胃口。
             int gIndex = 0;
             for(int i = 0; i < s.size(); i++)//饼干
             {
                 if(gIndex < g.size() && g[gIndex] <= s[i])
                 {
                     result++;
                     gIndex++;
                 }
     
             }
             return result;
         }
     };
     ```



​	总结：

​	1. 如果使用一重循环的时候，注意在判断中除了需要基本的判断条件外，还要注意gIndex/sIndex的合法性。





LC376

​	题目链接：[Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/)

​	解题思路：

​		**局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值**。

​		**整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列**。

​		因为题目要求最后返回的是子序列的长度，所以只需要统计峰值的数量就可以，不需要真的删除元素

		1. 考虑只有两个元素的情况。所以需要在判断条件的地方，加上preDiff == 0这一条件。这样可以保证两个元素的时候，返回为2
		1. 考虑只有一个元素的情况，也算是摆动序列，所以初始化为1.
		1. 上下坡中间有相等的情况，例如 [1,2,2,2,1]这样的数组。其实1的想法已经解决了这一问题，都是默认不考虑相同变量中的靠左的变量，保留最右边的一个。
		1. 这个最后一个情况很容易忽略，在做题中，也是test以后才找到。就是在单调上升/下降的过程中有相同的元素（水平区间）。我们只需要在 这个坡度 摆动变化的时候，更新 prediff 就行，这样 prediff 在 单调区间有平坡的时候 就不会发生变化，造成我们的误判。

```C++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int curDiff = 0;
        int preDiff = 0;
        int count = 1;
        for(int i = 0; i < nums.size() -1 ; i++)
        {
            curDiff = nums[i+1] - nums[i];
            if((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0))
            {
                count++;
                preDiff= curDiff;
            }

           
        }
        return count;
        
    }
};
```





LC53

​	题目链接：[Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

​	解题思路：

 1. 暴力解法超时（两个循环，外层循环确定起始位置，内层循环遍历，一直寻找以该起始位置开始的最大子序列的和）

 2. 这道题如果使用贪心来做，局部最优就是当前的连续和为负数的时候，就放弃，从下一个元素开始重新计算连续和，因为负数加下一个元素的连续和只会越来越小。与其带上这个拖累，不如从下一个元素开始重新计算连续和。

 3. 注意我的理解是不会出现比如前面n个元素之和刚刚变成负数，但是n个元素中的最后一个是正数，接下来要加的元素也是正数的情况。因为前面之和如果一直是正数，那么这个最后一个元素一定是负数，而且是很大的负数。如果前面这个最后一个元素是一个很大的正数，那么前面的n-1为负数，但是按照我们的贪心，这种情况早在之前的处理中就处理了，当第一次为负数的时候，就放弃了。

    ```C++ 
    class Solution {
    public:
        int maxSubArray(vector<int>& nums) {
            int result = INT_MIN;
            int count = 0;
            for(int i = 0; i < nums.size(); i++)
            {
                count += nums[i];
                if(count > result)
                {
                    result = count;
                }
                if(count <= 0)
                {
                    count = 0;
                }
    
            }
            return result;
            
        }
    };
    ```

    
