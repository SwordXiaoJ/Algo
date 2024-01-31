Day 37

​	LC738 

​	题目链接：[Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/)

​	解题思路：

​		如果遇到nums[i+1] 比 nums[i]小的情况，就把nums[i]--,nums[i+1]变为9.

​		但是如果从前往后遍历，例如332,会变成329.

​		所以考虑从后往前遍历，例如332，首先变成329，然后变成299

在第一个for循环寻找不满足递增条件的位置时，注意不能使用break; 因为要找到从前往后第一个不满足条件的位置，而不是从后往前第一个，所以需要去不断迭代更新。直到遍历完所有位。

```C++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string s = to_string(n);
        int nine_start = s.size();
        for(int i = s.size()-1; i > 0; i--)
        {
            if(s[i] < s[i-1])
            {
                s[i-1]--;
                nine_start = i;
            }
        }

        for(int i = nine_start; i < s.size(); i++)
        {
            s[i] = '9';
        }

        return stoi(s);
        
        
    }
};
```





LC968

​	题目链接：[Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

​	解题思路：

​		局部最优：叶子节点的父节点安装摄像头，覆盖范围最广。用的摄像头数量最少。

​		每个节点的状态有三个，有覆盖，无覆盖，有摄像头。空节点是有覆盖的状态。

1. 在 `left == 2 && right == 2` 的情况下，`result` 不增加。在这种情况下，当前节点的左右子节点都已经被覆盖了（可能是由下面的节点的摄像头覆盖的），但是当前节点自己并没有摄像头。这里的关键是，尽管当前节点自身没有被直接覆盖，我们也不会立即在这个节点上放置摄像头。原因在于我们可以更有效地利用摄像头。如果一个节点的父节点没有被覆盖，那么在父节点上放置摄像头是更有效的选择，因为它可以同时覆盖父节点本身和它的两个子节点。

​	所以，当 `left == 2 && right == 2`，我们返回 0，表示这个节点目前没有被覆盖。但我们不在这里放置摄像头，而是看看是否可以在上层节点（父节点）上放置摄像头以覆盖更多节点。



		2. 而在left == 0 || right == 0的情况下，这个点必须放，因为从底向上，底部已经没办法满足了，如果这个点不放，上面的更加满足不了。
		2. 最后处理完，可能存在根节点仍然未被覆盖的情况，需要进行一个判断。
		2. 注意  if(left == 1 || right == 1) return 2；不能在if(left == 0 || right == 0)前面，因为存在左右其中一个有摄像头，一个是无覆盖的情况。这种情况下，需要result++。	

```C++
 //0:无覆盖
 //1：有摄像头
 //2：有覆盖
class Solution {
private:
    int result;
    int traversal(TreeNode* cur)
    {
        if(cur == NULL) return 2; // 空节点

        int left = traversal(cur->left);
        int right = traversal(cur->right);

        //左右孩子都被覆盖，则这个节点本身是无覆盖状态
        if(left == 2 && right == 2) return 0;
        //左右节点至少有一个无覆盖的情况
        if(left == 0 || right == 0)
        {
            result++;
            return 1;
        }

        //左右孩子至少有一个有摄像头
        if(left == 1 || right == 1) return 2;

        return -1;



    }
public:
    int minCameraCover(TreeNode* root) {
        if(root->left == NULL && root->right == NULL) return 1;

        if(traversal(root) == 0)
        {
            result++;
        }
        return result;
        

        
    }
};
```

