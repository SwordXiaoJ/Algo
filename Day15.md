Day15 LC102



牢记层序遍历的模板，可以解决很多问题



LC102

​	题目链接：[Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

​	解题思路：

​	使用队列,一次AC。注意再循环每一层的过程中，要注意循环条件是使用固定的size，而不是que.size()。因为que.size()随着元素添加，是在变化的。



LC107 

​	题目链接：[Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)

​	使用模板，最后反转结果集即可



LC199

​	题目链接：[Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

​	使用模板，只不过每次加入结果集的是每一层的最后一个元素



LC637 

​	题目链接：[Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)

​	使用模板，只不过需要统计每一层的的数值之和，然后除以每一层的节点个数



LC429 

​	题目链接：[N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)

​	使用模板，只不过每个节点有多个子节点，所以需要使用遍历。



LC515

​	题目链接： [Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/)

​	使用模板，使用一个变量记录每一层的最大值就好



LC116

​	题目链接：[Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

​	使用模板，使用一个preNode记录每次遍历节点的前一个节点即可。



LC117

​	题目链接：[Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

​	和116做法完全一样



LC104

​	题目链接：[Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

​	使用模板，用一个变量，在每层遍历完之后记录一下长度即可

​	也可以使用递归来做，递归函数的返回值是depth，参数是node*,内部逻辑是如果是空节点，返回0。然后分别递归左右子树，取其中的最大值+1作为递归函数的返回值。





LC111

​	题目链接：[Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

 1. 迭代法

    ​	使用模板，使用一个变量来记录最小深度即可。注意，当遍历到第一个左右节点都是空的叶子结点的时候，直接返回就是最小深度。

 2. 递归，注意这个节点只有一边叶子的节点的时候，返回的是有叶子节点那一边的深度+1

    ```C++
    class Solution {
    public:
        int getDepth(TreeNode* node)
        {
            if(node == nullptr) return 0;
            int leftDepth = getDepth(node->left);
            int rightDepth = getDepth(node->right);
            if(node->left == NULL && node->right != NULL) return 1 + rightDepth;
            if(node->right == NULL && node->left != NULL) return 1 + leftDepth;
             
    
            return 1+ min(leftDepth,rightDepth);
        }
        int minDepth(TreeNode* root) {
            return getDepth(root);
            
            
        }
    };
    ```

    



LC226

​	题目链接：[Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

​	解题思路：

  1. 递归法（前序，后序）调整swap的位置即可。

     ```C++
     public:
         TreeNode* invertTree(TreeNode* root) {
             if(root == NULL) return root;
             
             invertTree(root->left);
             invertTree(root->right);
             swap(root->left,root->right);
     
             return root;
             
         }
     };
     ```

     

  2. 迭代法（层次遍历，深度遍历（使用统一写法的模板，稍加修改就可）

     深度优先模板

     ```C++
     TreeNode* invertTree(TreeNode* root) {
             stack<TreeNode*> stk;
     
             TreeNode* cur = root;
             while(cur)
             {
                 stk.push(cur);
                 cur = cur->left;
             }
             while(!stk.empty())
             {
                 TreeNode* node = stk.top();
                 stk.pop();
                 TreeNode* right = node->right;
                 while(right)
                 {
                     stk.push(right);
                     right = right->left;
                 }
             }
             
         }
     ```

     广度优先模板

     ```C++
     TreeNode* invertTree(TreeNode* root) {
             queue<TreeNode*> que;
             if(root != NULL) que.push(root);
     
             while(!que.empty())
             {
                 int size = que.size();
                 for(int i = 0; i < size; i++)
                 {
                     TreeNode* node = que.front();
                     que.pop();
             
                     if(node->left) que.push(node->left);
                     if(node->right) que.push(node->right);
                 }
             }
             return root;
             
         }
     ```

     

LC101

​	题目链接：[Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

​	解题思路：

对称是轴对称，所以比较的其实是所谓的内外侧节点是否相同。也就是以根节点开始，同时遍历两个子树，

  1. 递归法

     思考终止条件的时候，可以放到最简单的情况来考虑，比如一个节点，或者三个节点等。考虑四种情况作为终止条件

     思考递归的步骤的时候，可以必最简单的情况再加一层来考虑。比如根节点的左右子树传进去以后，要做一层递归（用一层递归函数），就是根节点的左右子树分别去调用递归函数，分别去判断相同，然后返回值，最后&&一下，就是根节点最终的结果。

     ```C++
     class Solution {
     private:
         bool compare(TreeNode* left,TreeNode* right)
         {
             if(left == NULL && right == NULL) return true;
             else if(left == NULL && right != NULL) return false;
             else if(left != NULL && right == NULL) return false;
             else if(left->val != right->val) return false;
     
             bool inSide = compare(left->right,right->left);
             bool outSide = compare(left->left,right->right);
             return inSide && outSide;
     
     
         }
     public:
         bool isSymmetric(TreeNode* root) {
             if(root == NULL) return true;
             return compare(root->left,root->right);
     
             
         }
     };
     ```

     

  2. 迭代法

     注意在左右都为空的时候，要用continue，不能直接return true.因为还要接下来判断其他子树，并不能直接像返回false一样直接返回。

     ```C++
     class Solution {
     public:
         bool isSymmetric(TreeNode* root) {
             if(root == NULL) return true;
             queue<TreeNode*> que;
             que.push(root->left);
             que.push(root->right);
     
             while(!que.empty())
             {
                 TreeNode* leftNode = que.front();
                 que.pop();
                 TreeNode* rightNode = que.front();
                 que.pop();
     
                 if(leftNode == NULL && rightNode == NULL) continue;//
     
                 if((leftNode == NULL && rightNode != NULL) || (leftNode != NULL && rightNode == NULL) || (rightNode->val != leftNode->val))
                 {
                     return false;
                 }
     
                 que.push(leftNode->left);
                 que.push(rightNode->right);
                 que.push(leftNode->right);
                 que.push(rightNode->left);
                 
             }
             return true;
     
             
         }
     };
     ```

     

​	



