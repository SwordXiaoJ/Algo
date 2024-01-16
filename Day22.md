Day 22 LC235,LC701



LC235

​	题目链接：[Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

​	解题思路：

​		二叉搜索树是有序的。如果一个节点是两个节点的公共祖先，那么这个节点一定是在这两个节点之间，即两个节点组成的闭区间的范围内。

​		注意p和q的大小关系不确定，所以需要加一个判断。

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL) return NULL;
        if(p->val < q->val)
        {
            if(root->val > q->val)
            {
                return lowestCommonAncestor(root->left,p,q);
            }
            else if(root->val < p->val)
            {
                return lowestCommonAncestor(root->right,p,q);
            }
            else // root的值在[]之间
            {
                return root;
            }
        }

        if(q->val < p->val)
        {
            if(root->val > p->val)
            {
                return lowestCommonAncestor(root->left,p,q);
            }
            else if(root->val < q->val)
            {
                return lowestCommonAncestor(root->right,p,q);
            }
            else // root的值在[]之间
            {
                return root;
            }
        }
        return root;
        
    }
};
```



​	迭代法(在这可以做优化，不用if else判断p和q的大小关系，从上面判断可知，分类以后，也是cur.val大于最大的，和小于最小的。但是大于最大的时候，一定也大于另一个值，所以只需要同时大于p和q即可)

```C++
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* cur = root;
        while(cur)
        {
            if(cur->val > p->val && cur->val > q->val)
            {
                cur = cur->left;
            }
            else if(cur->val < p->val && cur->val < q->val)
            {
                cur = cur->right;
            }
            else
            {
                return cur;
            }
        }
        return NULL;
    }
};
```

​	总结：

​		递归函数有返回值，如何区分要搜索一条边还是整棵树。

​		搜索一条边的写法：(找到就返回)

```C++
if (递归函数(root->left)) return ;
if (递归函数(root->right)) return ;
```

 		搜索整个树写法：（要去判断整棵树）

```C++
left = 递归函数(root->left);
right = 递归函数(root->right);
left与right的逻辑处理;
```



LC701

​	题目链接：[ Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)

​	解题思路：

  1. 直接利用原始函数作为递归函数，具有返回值，直接AC。

  2. 用没有返回值的递归函数，这样处理起来有点麻烦

  3. 利用二叉树的性质去迭代遍历

     ```C++
     TreeNode* insertIntoBST(TreeNode* root, int val) {
             TreeNode* cur = root;
             if(root == NULL)
             {
                 return new TreeNode(val);
             }
             TreeNode* parent = root;
             while(cur)
             {
                 parent = cur;
                 if(cur->val > val)
                 {
                     cur = cur->left;
                 }
                 else
                 {
                     cur = cur->right;
                 }
             }
             if(parent->val > val)
             {
                 parent->left = new TreeNode(val);
             }
             else
             {
                 parent->right = new TreeNode(val);
             }
             return root;
     
             
         }
     ```

     



LC 450

​	题目链接：[Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

​	解题思路：

  1. 递归来做，难点是左右孩子都不为空的时候。有两种方式，一种是题解中说的将删除节点的左子树头节点放在删除节点的右子树的最左面节点的左孩子上。（最左面节点的左孩子，就是右子树中最小的位置）。 另一种方式是反过来，将删除节点的右子树的头节点放在删除节点的左子树的最右面节点的右孩子上。（最右面节点的右孩子，是左子树中最大的位置）

      一次AC。



