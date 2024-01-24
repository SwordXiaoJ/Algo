Day 29 LC491,LC46,LC47



LC491

​	题目链接：[Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)

​	解题思路：

​		树层去重，树枝不去重

​		使用unordered_set去重，**`unordered_set<int> uset;` 是记录本层元素是否重复使用，新的一层uset都会重新定义（清空），所以要知道uset只负责本层！**

​		注意，不可以使用sort排序，会改变原来的数据顺序。

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,int startIndex)
    {
        if(path.size() > 1)
        {
            result.push_back(path);
        }

        if(startIndex >= nums.size())
        {
            return;
        }
        unordered_set<int> used; // 同一个父节点下的一层只需要一个，所以在for循环外面声明
        for(int i = startIndex; i < nums.size(); i++)
        {
            if(used.find(nums[i]) != used.end())
            {
                continue;
            }
            else if(!path.empty() && path[path.size() - 1] > nums[i])
            {
                continue;
            }



            path.push_back(nums[i]);
            used.insert(nums[i]);
            backtracking(nums, i+1);
            path.pop_back();

        }

    }
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
        
    }
};
```



LC46

​	题目链接：[Permutations](https://leetcode.com/problems/permutations/)	

​	解题思路：

​		组合问题不需要startIndex,因为每次都从头开始搜索，但是需要使用一个used数组来记录有哪些元素被使用了，一个排列里面一个元素只能使用一次。

​		

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,vector<bool>& used)
    {
        if(path.size() == nums.size())
        {
            result.push_back(path);
            return;
        }
        for(int i = 0; i < nums.size(); i++)
        {
            if(used[i] == true) continue;

            path.push_back(nums[i]);
            used[i] = true;
            backtracking(nums, used);
            used[i] = false;
            path.pop_back();
        }

    }
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> used(nums.size(),false);
        backtracking(nums,used);
        return result;
        
        
    }
};
```

LC47

​	题目链接：[Permutations II](https://leetcode.com/problems/permutations-ii/)

​	解题思路：

​		加入之前用过的树层去重逻辑即可。一次AC。

​		这个去重逻辑，写为true也可以去重，此时为树枝去重，效率低了很多，做了很多无用的搜索。但是必须要有true或者false的判断，不可以省略。省略了的话，一会是false,一会是true。不能得到答案。

```
if(i > 0 && nums[i] == nums[i-1] && used[i-1] == true)
            {
                continue;
            }
```



```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,vector<bool>& used)
    {
        if(path.size() == nums.size())
        {
            result.push_back(path);
            return;
        }
        for(int i = 0; i < nums.size(); i++)
        {
            //used[i-1] == true,说明是同一个树枝上使用过
            //used[i-1] == false，说明是同一个树层上使用过

            //如果同一个树层nums[i] == nums[i-1]且used为false,说明相同，则跳过
            if(i > 0 && nums[i] == nums[i-1] && used[i-1] == false)
            {
                continue;
            }
            if(used[i] == true) continue;
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<bool> used(nums.size(),false);
        backtracking(nums, used);
        return result;
        
    }
};
```

