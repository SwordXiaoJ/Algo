Day 52

​		LC198

​			题目链接：[House Robber](https://leetcode.com/problems/house-robber/)

​			解题思路：

​				很基本的dp模型，一次AC

​				

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 1) return nums[0];

        //dp[i]表示从下标为0-i的房屋内打劫，最多可以偷窃的金额为dp[i]
        vector<int> dp(nums.size());

        //递推公式,如果打劫这间房，则需要去看dp[i-2]; 如果不打劫这间房，则去考虑dp[i-1]
        //dp[i] = max(dp[i-2] + nums[i], dp[i-1]);

        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);

        for(int i = 2; i < nums.size(); i++)
        {
            dp[i] = max(dp[i-2] + nums[i], dp[i-1]);
        }





        return dp[nums.size()-1];
        
    }
};
```

​		LC213

​			题目链接：[House Robber II](https://leetcode.com/problems/house-robber-ii/)

​			解题思路：

​			连成环，必然首尾元素只能取其一，或者都不取。所以分为三种情况。

​			情况一： 首尾都不考虑

​			情况二：仅仅考虑首元素，不考虑尾元素

​			情况三：仅仅考虑尾元素，不考虑首元素

​			情况二和情况三其实已经包含了情况一。

​			注意，这边考虑和选不选首尾元素，是两个概念。考虑并不表示这要选取，只不过是列入考虑范围内，具体要不要，由递归公式决定。

​			偷的逻辑还是使用原来的逻辑，只不过传入的数组的起始和终止位置变了。

```C++
class Solution {
private:
    //左闭右闭
    int rob(vector<int>& nums, int start, int end)
    {
        if(start == end) return nums[start];

        //dp[i]表示从下标为0-i的房屋内打劫，最多可以偷窃的金额为dp[i]
        vector<int> dp(end+1);

        //递推公式,如果打劫这间房，则需要去看dp[i-2]; 如果不打劫这间房，则去考虑dp[i-1]
        //dp[i] = max(dp[i-2] + nums[i], dp[i-1]);

        dp[start] = nums[start];
        dp[start+1] = max(nums[start], nums[start+1]);

        for(int i = start + 2; i <= end; i++)
        {
            dp[i] = max(dp[i-2] + nums[i], dp[i-1]);
        }

        return dp[end];
        

    }
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 1) return nums[0];
        int result1 = rob(nums,0,nums.size()-2);
        int result2 = rob(nums,1,nums.size()-1);
        return max(result1,result2);
        
    }
};
```

​		

LC337

​	题目链接：[House Robber III](https://leetcode.com/problems/house-robber-iii/)

​	解题思路：

​		如果抢了当前节点，两个孩子就不能动，如果没抢当前节点，就可以考虑抢左右孩子（**注意这里说的是“考虑”**）

​		所以应该是后序遍历比较合理。



​		1. 直接暴力解法，会超时

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        if(root == NULL) return 0;
        

        int val1 = root->val;
        if(root->left) val1 += rob(root->left->left) + rob(root->left->right);
        if(root->right) val1 += rob(root->right->left) + rob(root->right->right);

        int val2 = rob(root->left) + rob(root->right);

        return max(val1,val2);
        
    }
};
```

  2. DP

     每个节点起始就是两个状态，偷或者不偷。用一个长度为2的一维dp数据就可以表示。dp[0]表示不偷当前节点所获得的最大金钱，dp[1]表示偷当前节点所获得的最大金钱。

     这里，因为递归是一层一层，所以递归过程中，每一层都会有一个dp数组，当前层的dp数组，就表示当前层遍历的节点的状态。

​	

​	递归三部曲

  1. 递归函数,返回的是当前节点偷或者不偷的结果

      vector<int> robtree(TreeNode* root);

  2. 终止条件

     ```
      if(cur == NULL)
             {
                 vector<int> result(2,0);
                 return result;
             }
     ```

  3. 遍历

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
private:
    // 长度为2的数组，0：不偷，1：偷
    vector<int> robTree(TreeNode* cur)
    {
        //如果该节点为空，偷和不偷的结果都一样，都是0
        if(cur == NULL)
        {
            vector<int> result(2,0);
            return result;
        }

        vector<int> left = robTree(cur->left);
        vector<int> right = robTree(cur->right);


        //偷当前节点
        int value1 = cur->val + left[0] + right[0];
        //不偷当前节点，那么可以偷也可以不偷左右节点，则取较大的情况
        int value2 = max(left[0], left[1]) + max(right[0], right[1]);

        return {value2, value1};

    }
public:
    int rob(TreeNode* root) {
        vector<int> result = robTree(root);
        return max(result[0],result[1]);
        
    }
};
```

