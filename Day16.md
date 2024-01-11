Day15 LC102



LC104

​	题目链接：[Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

​	解题思路：

  1. 递归法

       （1） 使用高度的概念来完成最大深度的求解，根节点的高度就是树的最大深度。其实就是在递归函数中，首先去让左右子树分别去求解最大高度，然后1+其中的最大值就是根节点的最大高度。这个版本的代码简洁。

     ```C++
     class Solution {
     private:
         int getDepth(TreeNode* root)
         {
             if(root == NULL) return 0;
             int leftDepth = getDepth(root->left);
             int rightDepth = getDepth(root->right);
             return 1+max(leftDepth,rightDepth);
         }
     public:
         int maxDepth(TreeNode* root) {
             return getDepth(root);
             
         }
     };
     ```

      （2）使用深度的概念来完成（需要使用回溯）。根节点的深度就是1，求最大深度的过程，就是有一个遍历去记录，然后去不断Update的过程。设置一个全局变量，这样最方便操作。然后每次需要把当前遍历的深度传给递归函数，递归函数每次需要+1以后，传给下一层，一直到叶子节点。

     ```C++
     class Solution {
     private: 
         int result = 0;
         void getDepth(TreeNode* root,int depth)
         {
             if(depth > result)
             {
                 result = depth;
             }
             if(root == NULL) return;
             if(root->left ==NULL && root->right == NULL) return;
             getDepth(root->left,depth+1);
             getDepth(root->right, depth+1);
             return;
             
     
         }
     public:
         int maxDepth(TreeNode* root) {
             if(root == NULL) return 0;
             int depth = 1;
             getDepth(root, depth);
             return result;
             
         }
     };
     ```

     

     

  2. 迭代法

     层序遍历模板+遍历记录



​	总结：

		1. 深度：任意一个节点到根节点的距离，深度是从上往下，深度在这里是从1开始。深度想象一个馕坑，是往下的，只有往下才有深度的概念。求深度，用的是**前序遍历**
		1. 高度：任意一个节点到叶子节点的距离。往上，才有高度的概念，从下往上。求高度用的是**后序遍历**，因为是从下往上去计数
		1. 本题是求最大深度，其实反过来也是求这个树的根节点的高度。**根节点的最大高度就是树的最大深度**





LC111

​	题目链接：[Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

​	解题思路：
  1. 迭代法

     层序遍历模板+变量记录

  2. 递归法（中代表的是处理逻辑模块）

     （1）高度的概念（使用的仍然是后序遍历）

     ​	根节点的最小高度（距离最近的叶子节点的距离）就是树的最小深度。

       

     ```C++
     class Solution {
     private:
         int getMinDepth(TreeNode* root)
         {
             if(root == NULL) return 0;
            if(root->left == NULL && root->right != NULL)
            {
                return 1+getMinDepth(root->right);
            }
            if(root->right == NULL && root->left != NULL)
            {
                return 1+getMinDepth(root->left);
            }
             
             int leftDepth = getMinDepth(root->left);
             int rightDepth = getMinDepth(root->right);
             return 1+min(leftDepth,rightDepth);
     
         }
         
     public:
         int minDepth(TreeNode* root) {
             if(root == NULL) return 0;
             return getMinDepth(root);
             
         }
     };
     ```
     
     
     （2）深度的概念

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
    int result;
    void getDepth(TreeNode* node, int depth)
    {
        if(node == NULL) return;
        if(node->left == NULL && node->right == NULL)
        {
            result = min(result,depth);
        }

        getDepth(node->left, depth+1);
        getDepth(node->right, depth+1);
        return;


    }
public:
    int minDepth(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        result = INT_MAX;
        int depth = 1;
        getDepth(root, depth);
        return result;
    }
};
```

LC222

​	题目链接：[Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

​	解题思路：

  1. 迭代法，层次遍历或者深度遍历，加一个变量来统计数量

  2. 递归法

     ```C++
     class Solution {
     private:
         int getCounts(TreeNode* node)
         {
             if(node == NULL) return 0;
             int leftNum = getCounts(node->left);
             int rightNum = getCounts(node->right);
             return 1+leftNum+rightNum;
     
         }
     public:
         int countNodes(TreeNode* root) {
             if(root == NULL) return 0;
             return getCounts(root);
             
         }
     };
     ```



3. 使用完全二叉树的性质，完全二叉树在满二叉树的情况下，可以用公式2^深度-1来计算，根节点的深度为1。

   所以可以分别递归左右孩子，一直到满二叉树为止，然后利用满二叉树的计算公式计算，然后递归一层一层退回去。

   如何判断满二叉树？ 左右递归遍历的深度相同。左边一直left->left,右边一直right->right.

   ```C++
   class Solution {
   private:
       int getCounts(TreeNode* node)
       {
           if(node == NULL) return 0;
           TreeNode* left = node->left;
           TreeNode* right = node->right;
           int leftDepth = 0;
           int rightDepth = 0;
           while(left)
           {
               left = left->left;
               leftDepth++;
           }
           while(right)
           {
               right = right->right;
               rightDepth++;
           }
           if(leftDepth == rightDepth)
           {
               return std::pow(2,leftDepth) - 1;
           }
           return 0;
         
        
   
       }
   public:
       int countNodes(TreeNode* root) {
           if(root == NULL) return 0;
           return 1+ countNodes(root->left)+countNodes(root->right);
           
       }
   };
   ```

   
