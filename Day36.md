Day 36

​	LC435

​	题目链接：[Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

​	解题思路：
​		一次AC

​		按照左边界排序，开始遍历数组，如果重叠，结果+1，且i+1的右边界更新为较小的那个值

 

```C++
class Solution {
private:
    static bool cmp(vector<int>&a, vector<int>&b)
    {
        return a[0] < b[0];    
    }
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        int result = 0;
        for(int i = 0; i < intervals.size()-1; i++)
        {
            if(intervals[i][1] > intervals[i+1][0]) // 重叠了
            {
                result++;
                intervals[i+1][1] = min(intervals[i][1],intervals[i+1][1]);
            }
        }
        return result;
        
    }
};
```





LC56

​	题目链接：[Merge Intervals](https://leetcode.com/problems/merge-intervals/)

​	解题思路：

​		一次AC，主要也是使用左边界进行排序

​		我这个解法最后需要处理一下末尾元素，单独把末尾元素加进入。

​		也可以选择和题解一样，先把第一个元素加进去，然后从1开始遍历。更改的时候，直接在result.back()上面进行更改就可以，代码可能简洁一些。

```C++
class Solution {
private:
    static bool cmp(vector<int>&a, vector<int>&b)
    {
        return a[0] < b[0];
    }
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        vector<vector<int>> result;
        if(intervals.size() == 0) return result;
        for(int i = 0; i < intervals.size()-1; i++)
        {
            if(intervals[i][1] >= intervals[i+1][0]) // 重叠
            {
                intervals[i+1][1] = max(intervals[i][1],intervals[i+1][1]);
                intervals[i+1][0] = min(intervals[i][0],intervals[i+1][0]); 
            }
            else
            {
                result.push_back(intervals[i]);
            }

        }
        result.push_back(intervals[intervals.size()-1]);
        return result;
        
    }
};
```





LC763

​	题目链接：[Partition Labels](https://leetcode.com/problems/partition-labels/)

​	解题思路：

​		题目如果使用回溯法来做，回溯法在处理过程中如何区分是否满足条件比较难写。

​		考虑先遍历一遍，统计每个字母的最远距离，然后重新遍历，使用一个right来表示最远距离，先获取一个最远距离，然后在到达这个最远距离之前的遍历中不断去更新它。一直到达到最远距离，是一个分割点。

 		更新它的原因是在到达最远距离的过程中有可能会出现新的字母，它的最远距离更远。

```C++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int hash[26] = {0};
        for(int i = 0; i < s.size(); i++) // 找到每个字母出现的最远位置
        {
            hash[s[i] -'a'] = i;
        }
        int left = 0;
        int right = 0;
        vector<int> result;
        for(int i = 0; i < s.size(); i++)
        {
            right = max(right,hash[s[i] - 'a']); // 在遍历的过程中，在到达right之前，不断去更新right,因为有可能在到达right之前的字符串中，会有新的更远的距离。
            if(i == right)
            {
                result.push_back(right - left + 1);
                left = i+1;
                right = 0;
                
            }
        }
        return result;
        
    }
};
```

