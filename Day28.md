Day 28 LC78,LC90,LC93





LC78

​	题目链接：[Subsets](https://leetcode.com/problems/subsets/)



​	总结：
​		1.  子集也是一种组合问题，因为它的集合是无序的，子集{1,2} 和 子集{2,1}是一样的。

**那么既然是无序，取过的元素不会重复取，写回溯算法的时候，for就要从startIndex开始，而不是从0开始！**

2. **与组合问题和分割问题不同，子集问题是求取树的所有节点。分割问题和组合问题是求取树的叶子节点**。

​	

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,int startIndex)
    {   
        result.push_back(path);
        if(startIndex >= nums.size())
        {
            return;
        }

        for(int i = startIndex; i < nums.size(); i++)
        {
            path.push_back(nums[i]);
            backtracking(nums, i+1);
            path.pop_back();
        }

    }
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
        
    }
};
```



LC90

​	题目链接：[Subsets II](https://leetcode.com/problems/subsets-ii/)

​	解题思路：

​		一次AC，就是加了一个树层去重的逻辑。可以使用used数组来去重，也可以直接使用下标来去重。



LC93

​	题目链接：[Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

​	解题思路：

​		一次AC

​	

```C++
class Solution {
private:
    vector<string> result;
    void backtracking(string& s,int startIndex,int pointNum)
    {
        if(pointNum == 3)
        {
            if(isValid(s, startIndex,s.size()-1))//保证最后一段字符串合法
            {
                result.push_back(s);
            }
            return;
        }

        for(int i = startIndex; i < s.size(); i++)
        {
            if(isValid(s, startIndex,i))
            {
                s.insert(s.begin()+i+1,'.');
                pointNum++;
                backtracking(s, i+2,pointNum);
                pointNum--;
                s.erase(s.begin()+i+1);
            }
            else
            {
                break;
            }
        }

    }

    bool isValid(string& s,int start,int end) // 左闭右闭
    {
        if(start > end)
        {
            return false;
        }
        if(s[start] == '0' && start != end)
        {
            return false;
        }
        int num = 0;
        for(int i = start; i <= end; i++)
        {
            if(s[i] > '9' || s[i] < '0') // 非法字符
            {
                return false;
            }
            num = num * 10 + (s[i] - '0');
            if(num > 255)
            {
                return false;
            }
        }


        
        return true;



    }

public:
    vector<string> restoreIpAddresses(string s) {
        backtracking(s, 0,0);
        return result;
        
    }
};
```



​	总结：

1. 在 C++ 中，使用 `std::string` 的 `insert` 方法在字符串的指定位置插入一个字符后，该位置及其之后的所有字符都会向后移动。新插入的字符会占据指定的位置，而原来在该位置及其后面的字符都会相应地向后移动一个位置。

  2. 在 C++ 中，使用 `std::string` 的 `erase` 方法删除字符串中的一个或多个字符后，所有位于被删除字符之后的字符都会向前移动以填补被删除的位置。

  3.  stoi(str)可以将str转为int,但是它的返回值不是一个特定的值，而是可能在不能处理的情况（例如有异常字符）的情况下抛出异常，所以没办法用返回值来使用它。

  4.  **本题中我第一次test没过是在isValid中，没有判断start > end的情况。这种情况，在最后一个字符段（剩余的字符组成）的判断合理性的情况下，是有可能发送的。此时startIndex指向字符串末尾字符的后一位，end指向末尾字符。**

​     
