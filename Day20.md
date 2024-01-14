Day 20 LC654,LC617,LC700,LC98

​	LC654

​		题目链接：[Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

​		解题思路：
​			1. 使用Day18的思路，考虑使用引用传递+下标的方式构建递归函数。主要注意就是递归函数的结束条件，如果空节点进入递归函数，则需要在终止条件的时候，考虑空节点的判断。如果在终止条件的时候不考虑空节点的判断（使用例如叶子节点作为递归函数终止条件），则需要在调用递归逻辑的时候，控制空节点不进入递归。

​		总结：

   1. **注意类似用数组构造二叉树的题目，每次分隔尽量不要定义新的数组，而是通过下标索引直接在原数组上操作，这样可以节约时间和空间上的开销。**

   2. **一般情况来说：如果让空节点（空指针）进入递归，就不加if，如果不让空节点进入递归，就加if限制一下， 终止条件也会相应的调整。**

      

​	LC617

​		题目链接：[Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

​		解题思路：

			1. 递归法，没有什么难度，主要考虑空节点是否进入递归的过程，涉及到不同的做法。
			1. 使用Queue,使用类似层次遍历的思路，只不过要判断节点是否为空的一些情况。两个树的节点依次加入queue.



​	LC700

​		题目链接：[Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

​		解题思路：

			1. 递归直接AC。
			1. 使用二叉搜索树的特性，左子树的节点值小于根节点，右子树的节点值大于根节点。利用一个指针去遍历即可。



​	LC98

​		题目链接：[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

  1. 递归/迭代中序遍历转换为数组，判断数组是不是单调递增的（严格单调）

  2. 在遍历过程中来不断判断，可以使用一个节点记录之前遍历的上一个节点的值。只要当前的节点的值比上一个大，就继续，遇到不满足条件的，返回false

     使用递归

     ```C++
     class Solution {
     public:
         TreeNode* pre = NULL;//记录前一个节点
         bool isValidBST(TreeNode* root) {
             if(root == NULL) return true;
             bool left = isValidBST(root->left);
             
             if(pre != NULL)//不等于NULL的时候才开始比较，等于NULL的时候是第一个节点（最左下角的节点）
             {
                 if(pre->val >= root->val)
                 {
                     return false;
                 }
             }
             pre = root;
     
             bool right = isValidBST(root->right);
     
             return left&&right;
             
         }
     };
     ```

     使用迭代

     ```C++
     class Solution {
     public:
     
         bool isValidBST(TreeNode* root) {
             stack<TreeNode*> stk;
             TreeNode* cur = root;
             TreeNode* pre = NULL;
             while(cur)
             {
                 stk.push(cur);
                 cur = cur->left;
             }
     
             while(!stk.empty())
             {
                 TreeNode* node = stk.top();
                 stk.pop();
                 if(pre != NULL)
                 {
                     if(pre->val >= node->val)
                     {
                         return false;
                     }
                 }
                 pre = node;
     
                 TreeNode* right = node->right;
                 while(right)
                 {
                     stk.push(right);
                     right = right->left;
                 }
             }
             return true;
             
         }
     };
     ```

     

