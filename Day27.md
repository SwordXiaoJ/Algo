Day 27 LC39,LC40,LC131



LC39

​	题目链接：[Combination Sum](https://leetcode.com/problems/combination-sum/)

​	解题思路：

​		使用递归模板，注意可以重复选取元素，所以在递归的时候，传递startIndex的时候，传递为i，不是i+1

​		剪枝优化：

​			如果不在for循环部分进行剪枝，目前的剪枝sum>target的时候，要进入到递归函数里面，才会进行剪枝。其实是多走了一层。

​			可以在for循环的时候，进行判断，如果下一层加入以后的sum会大于target，就根本不会进入下一层递归。**但是注意这种情况下，要对集合进行排序，从小到大。**

​		

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates,int target,int curSum,int startIndex)
    {
        //version1
        if(curSum > target)
        {
            return;
        }
        if(curSum == target)
        {
            result.push_back(path);
            return;
        }

        for(int i = startIndex; i < candidates.size(); i++)
        //for(int i = startIndex; i < candidates.size() && curSum + candidates[s] <= target; i++) version2
        {
            path.push_back(candidates[i]);
            curSum += candidates[i];
            backtracking(candidates,target,curSum,i);
            curSum -= candidates[i];
            path.pop_back();
        }


    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        backtracking(candidates,target,0,0);
        return result;
        
    }
};
```



​	总结：

  1. 如果是一个集合来求组合的话，就需要startIndex。如果是多个集合取组合，各个集合之间相互不影响，那么就不用startIndex。

     



LC40

​	题目链接：[Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

​	解题思路：

​		同一层树的元素之间不能重复用，但是同一个树枝上的元素是可以重复的。因为同一个树枝上的元素构成一个组合，这个组合内部的元素是可以重复的（只要原始的里面有这个数量的重复元素）

​		**树层去重的话，需要对数组排序！**



​		1. 没有使用used数组，直接使用index来去重，不过注意去重的逻辑

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates,int target,int curSum, int startIndex)
    {
        if(curSum == target)
        {
            result.push_back(path);
            return;
        }

        for(int i = startIndex; i < candidates.size() && curSum + candidates[i] <= target; i++)
        {
            if(i > startIndex && candidates[i] == candidates[i-1]) // 第一个元素不能去重，因为第一个元素去重，可能会在下面的选择中，直接就漏掉很多情况，蔽日1，2，2，2，5. target = 5.如果第一个元素去重，在递归过程中，2就会只算一个，然后后面的一直被去重。所以注意第一个元素不是0，而是startIndex
            {
                continue;
            }
            curSum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target,curSum,i+1);
            curSum -= candidates[i];
            path.pop_back();

        }
    }

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        backtracking(candidates, target,0,0);
        return result;

        
        
    }
};
```



​	 2. 使用used数组，如果是同一个树枝上的，那么used[i-1] == true,如果是同一个树层上的，因为是回溯回去的，所以used[i-1] == false.

​	

```
class Solution {
private: 
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target,int curSum, int startIndex,vector<bool> &used)
    {
        if(curSum == target)
        {
            result.push_back(path);
            return;
        }
        for(int i = startIndex; i < candidates.size() && curSum + candidates[i] <= target; i++)
        {
            if(i > startIndex && candidates[i-1] == candidates[i])
            {
                if(used[i-1] == false)//说明是同一层的
                {
                    continue;
                }
                else // used[i] == true 说明是一个树枝上的
                {
                }
            }

            curSum += candidates[i];
            path.push_back(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, curSum, i + 1, used);
            path.pop_back();
            curSum -= candidates[i];
            used[i] = false;

        }

    }
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used(candidates.size(),false);
        sort(candidates.begin(),candidates.end());
        backtracking(candidates,target,0,0,used);
        
        return result;
        
    }
};
```



LC131

​	题目链接：[Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

​	解题思路：

​		字符串切割和集合很像，从aaab中进行切割，就是比如先单独切割出来a,然后在剩下的aab中递归进行切割。树层就是aaab中首先切割出aa,然后aaa,然后aaab。

​		所以切割位置就是回溯函数的一个变量。切割的终止条件就是切割位置已经在字符串末尾了。

​		比较容易犯错的是子串选取的那个参数的设置。 

```
string subStr = s.substr(startIndex,i-startIndex+1);
```

​		判断回文子串使用的是双指针法。

```
class Solution {
private:
    vector<vector<string>> result;
    vector<string> path;
    void backtracking(string& s,int startIndex)
    {
        if(startIndex >= s.size())
        {
            result.push_back(path);
            return;
        }

        for(int i = startIndex; i < s.size(); i++)
        {
            string subStr = s.substr(startIndex,i-startIndex+1);
            if(isPalind(subStr))
            {
                path.push_back(subStr);
            }
            else
            {
                continue;
            }
            backtracking(s, i+1);
            path.pop_back();
        }

    }

    bool isPalind(string &s)
    {
        int start = 0;
        int end = s.size() - 1;
        while(start <= end)
        {
            if(s[start] != s[end])
            {
                return false;
            }
            start++;
            end--;
        }
        return true;

    }
public:
    vector<vector<string>> partition(string s) {
        backtracking(s, 0);
        return result;
        
    }
};
```

