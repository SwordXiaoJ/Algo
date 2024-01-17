Day 22 LC669,LC108,LC538



LC669



​	 题目链接：[Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

​	解题思路：

  1. 递归法，但是注意删除操作很巧妙。不能直接判断不满足条件，就返回NULL。要考虑节点之间删除以后的连接关系。

     ```C++
     class Solution {
     public:
         TreeNode* trimBST(TreeNode* root, int low, int high) {
             if(root == NULL) return NULL;
     
             if(root->val > high)
             {
                 return trimBST(root->left, low, high);
     
             }
             if(root->val < low)
             {
                 //直接不去考虑这个点的左子树，因为这个点的左子树比它小，肯定都小于low了，都要删除掉
                 //要去递归考虑这个点的右子树,在右子树中做完删减，然后返回右子树给上层。
                 return trimBST(root->right, low, high);
             }
     
             root->left = trimBST(root->left, low, high); // 接住if(root->val < low的返回值)
             root->right = trimBST(root->right,low,high); //同理
             return root;
             
         }
     };
     ```

     

  2. 迭代法

      首先，我们要去判断它的根节点是否满足要求。如果根节点不满足要求，我们就要去移动，直到根节点满足要求，然后去依次修建它的左右子树。

       左子树的修剪过程：

     	1. node的左子树为空，不需要修建。
      	2. node的左子树非空
          	1. 如果它的node->left的值小于low，那么node->left和它的左子树都不符合要求，设置node->left = node->left-right。然后再重新去判断并修剪node->left.
          	2. 如果node->left的值大于等于low,又因为node的值此时满足条件，所以node->left->right一定满足条件，此时node = node->left,然后重新对node的左子树进行判断与修剪。

     ```C++
     class Solution {
     public:
         TreeNode* trimBST(TreeNode* root, int low, int high) {     
             while(root)
             {
                 if (root->val < low) 
                 {
                     root = root->right;
                 } 
                 else if(root->val > high)
                 {
                     root = root->left;
                 }
                 else
                 {
                     break;
                 }
             }
             if(root == NULL) return NULL;
             TreeNode* cur = root;
             while(cur->left)
             {
                 if(cur->left->val < low)
                 {
                     cur->left = cur->left->right;
                 }
                 else
                 {
                     cur = cur->left;
                 }
             }
             cur = root;
             while(cur->right)
             {
                 if(cur->right->val > high)
                 {
                     cur->right = cur->right->left;
                 }
                 else
                 {
                     cur = cur->right;
                 }
             }
             return root;  
         }
     };
     ```

     

​	



LC108

​	题目链接：[Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

​	解题思路：

1. 递归法，很简单，取中间元素就可以。坚持不变量原则，以及注意使用函数返回值就可以。

		2. 迭代法：太麻烦了





LC538

​	题目链接：[Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)

​	解题思路：

​		1. 二叉搜索树，是有序的，而且从图中可以看出，从最右下角的元素开始累加就好。遵循的顺序是右中左。然后因为要累加，使用一个pre记录前一个元素，就可以。一次AC

​	

```C++
class Solution {
public:
    TreeNode* pre = NULL;
    TreeNode* convertBST(TreeNode* root) {
        if(root == NULL) return NULL;
        convertBST(root->right);
        if(pre == NULL)
        {
            root->val += 0;
        }
        else
        {
            root->val += pre->val;
        }
        pre = root;

        convertBST(root->left);

        return root;
        
    }
};
```



  2. 迭代法：使用深度遍历的模板，只不过需要改一下顺序（right和left交换一下）。然后用一个pre记录当前遍历节点的前一个结点就好。

     ```C++
     class Solution {
     public:
         TreeNode* convertBST(TreeNode* root) {
             stack<TreeNode*> stk;
             if(root == NULL) return NULL;
             TreeNode* cur = root;
             TreeNode* pre = NULL;
             while(cur)
             {
                 stk.push(cur);
                 cur = cur->right;
             }
             while(!stk.empty())
             {
                 TreeNode* node = stk.top();
                 stk.pop();
                 if(pre != NULL)
                 {
                     node->val += pre->val;
                 }
                 TreeNode* left = node->left;
                 while(left)
                 {
                     stk.push(left);
                     left = left->right;
                 }
                 pre = node;
             }
             return root;
             
         }
     };
     ```

     
