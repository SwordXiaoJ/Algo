Day 21,LC530,LC501,LC236

​	LC530

​		题目链接：[Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

​		解题思路：

​			和昨天的题一样，利用二叉搜索树的性质转为数组进行计算，或者使用边遍历便计算的方法。两种思路，都可以使用迭代或者递归。		

​			直接AC



​	  LC501

​		  题目链接：[Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/)

​		  解题思路：

   1. 随便一种遍历方式，统计节点的值的数量，然后排序，并输出。

      map不可以直接使用sort函数，sort函数只能用于vector或者deque. map的排序，需要先根据map创建（将map中的元素复制到）一个vector中，然后进行排序

      ```C++
       vector<pair<int,int>> vector(map.begin(),map.end());
      ```

      ```C++
       bool static cmp() //自定义比较函数
          {
      
          }
      ```

      

   2. 利用二叉搜索树的特性

       它的遍历过程，本质上是个有序数组的遍历过程。可以考虑引入pre这个变量，一开始为NULL。后面初始化为最左下角的第一个节点。如果是一个数组，做题的方法一般常规思路是先遍历数组找出最大的频率maxCount，然后再遍历一遍，把出现频率为maxCount的元素放进去。但是其实可以同时存在maxCount和count两个元素，只不过需要在maxCount == count的时候把元素放进去，但是需要在count > maxCount的时候，更新maxCount，并且把原来的结果集清空，添加新的元素。

      ```C++
      class Solution {
      private:
          vector<int> result;
          int count = 0;
          int maxCount = 0;
          TreeNode* pre = NULL;
          void getMode(TreeNode* node)
          {
              if(node == NULL) return;
      
              getMode(node->left);
              if(pre == NULL)// 这是最左下角的节点，也是第一个处理的节点
              {
                  count = 1;
              }
              else
              {
                  if(node->val == pre->val)
                  {
                      count++;
                  }
                  else
                  {
                      count = 1;
                  }
              }
              if(count == maxCount)
              {
                  result.push_back(node->val);
              }
              if(count > maxCount)
              {
                  maxCount = count;
                  result.clear();
                  result.push_back(node->val);
              }
              pre = node;
              getMode(node->right);
              return;
              
              
      
          }
      public:
          vector<int> findMode(TreeNode* root) {
      
              getMode(root);
              return result;
              
          }
      };
      ```

      

​	LC236

​	题目链接：[Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

​	解题思路：

​		使用回溯法，从下向上查找。去左子树找p或者q,去右子树也找p或者q，找到以后就返回p或者q，如果左右子树都不为空，则中为结果。（情况一）

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL) return NULL;
        if(root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q); // left告诉我们左子树有没有出现p或者q
        TreeNode* right = lowestCommonAncestor(root->right, p, q); //同理
        if(left != NULL && right != NULL) return root;
        if (left == NULL && right != NULL) return right;
        else if (left != NULL && right == NULL) return left;
        else  
        { //  (left == NULL && right == NULL)
            return NULL;
        }


        
    }
};
```

​		情况二，如果p或者q有一个节点本身就是公共祖先，但是处理逻辑其实是重合的。

​		就是遇到p或者q就直接返回，甚至不需要判断这个先遇到节点的下面的那些节点的情况。直接去返回就可以了。不明白就画一个最简单的情况就好

​	总结：

		1. 二叉树如何自底向上查找？回溯。后序遍历是天然的回溯。可以根据左右子树的返回值，来处理中间节点的值。
		1. 注意不可能左子树出现p,右子树也出现p,因为每一个node.val是唯一的，且p不等于q,题目要求。
