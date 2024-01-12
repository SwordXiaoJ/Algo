Day18 LC513,LC112,LC106



LC513

​	题目链接：[Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)

​	解题思路：

		1. 最左下角就是深度最深那一层的第一个，用层序遍历直接解决。
		1. 使用递归，但是注意因为result需要遍历整个树来不断更新，所以递归函数不需要返回值，设一个全局变量。



总结：

​	1. 如果递归函数不需要遍历整棵树，设置一个返回值，用于判断。如果需要便利整棵树，就设置一个全局变量，用以不断更新即可，不用设置返回值。

​		

- 如果需要搜索整棵二叉树且不用处理递归返回值，递归函数就不要返回值。（这种情况就是本文下半部分介绍的113.路径总和ii）
- 如果需要搜索整棵二叉树且需要处理递归返回值，递归函数就需要返回值。 （这种情况我们在[236. 二叉树的最近公共祖先 (opens new window)](https://programmercarl.com/0236.二叉树的最近公共祖先.html)中介绍）
- 如果要搜索其中一条符合条件的路径，那么递归一定需要返回值，因为遇到符合条件的路径了就要及时返回。（本题的情况）



LC112

​	题目链接：[Path Sum](https://leetcode.com/problems/path-sum/)

​	解题思路：

  1. 迭代法

      用统一的模板加修改，注意的是while(right)循环中，每次添加新的节点进去以后，**要在结尾重新更新一下node**,不然node依然指向pop出去的节点，会产生计算错误。

     ```C++
     class Solution {
     public:
         bool hasPathSum(TreeNode* root, int targetSum) {
             if(root == NULL) return false;
             stack<pair<TreeNode*,int>> stk;
             stk.push(pair<TreeNode*,int>(root,root->val));
             TreeNode* cur = root->left;
             while(cur)
             {
                pair<TreeNode*,int> item = stk.top();
                stk.push(pair<TreeNode*,int>(cur,item.second+cur->val));
                cur = cur->left;
             }
     
             while(!stk.empty())
             {
                 pair<TreeNode*,int> node = stk.top();
                 stk.pop();
     
                 if(node.first->left == NULL && node.first->right == NULL && node.second == targetSum)
                 {
                     return true;
                 }
                 TreeNode* right = node.first->right;
                 while(right)
                 {   
                     stk.push(pair<TreeNode*,int>(right,node.second+right->val));
                     right = right->left;
                     node = stk.top();
                 }
             }
             return false;      
         }
     };
     ```

     

  2. 递归法

     在递归函数中，if(node->left)和if(node->right)中，不能直接return递归函数返回值，因为是寻找true的过程，所以true的时候返回，false的时候，要继续去判断。

     ```C++
     class Solution {
     private:
         bool pathSum(TreeNode* node,int curSum)
         {
            
             if(node->left == NULL && node->right == NULL)
             {
                 if(curSum == 0)
                 {
                     return true;
                 }
                 else
                 {
                     return false;
                 }
             }
             if(node->left)
             {
                 if(pathSum(node->left, curSum-node->left->val)) return true;
             }
             if(node->right)
             {
                 if(pathSum(node->right, curSum-node->right->val)) return true;
             }
             return false;
             
     
         }
     public:
         bool hasPathSum(TreeNode* root, int targetSum) {
             if(root == NULL ) return false;
             return pathSum(root,targetSum-root->val);
     
             
         }
     };
     ```



Additional : LC113

​	题目链接：[Path Sum II](https://leetcode.com/problems/path-sum-ii/)

​	解题思路：

​		和之前找到所有的路径类似，使用迭代法会很麻烦，类似于手动实现回溯，所以直接使用递归算法。

​	      递归法

​		直接AC，没有什么难度。

​		这道题特殊在path值传递和引用传递都可以过，引用传递的时候，效率高一些。也可以设置全局变量，都可以。





LC106

​	题目链接：[Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

​	解题思路：

​		使用递归的思路。

		1. 根据后序遍历的最后一个元素，作为切割点的值，新建一个节点，这是根节点。去中序遍历的数组中寻找它对应的下表，找到中序遍历数组中的切割位置。
		1. 将中序数组切割为左右两部分，并根据对应的长度切割后续数组。
		1. 递归，root->left 赋值为新的左半部分的中序和后序数组，root->right同理。
		1. 返回root



​	**注意终止条件是后续遍历数组或者中序遍历数组大小为0.**

```C++
class Solution {
private:
    TreeNode* build(vector<int>& inorder, int inorderStart, int inorderEnd, vector<int>& postorder, int postorderStart, int postorderEnd)
    {
        //1. base case
        //如果postorder数组大小为0，说明里面是空节点了。
        if (postorderStart == postorderEnd || inorderStart == inorderEnd) return NULL;
       
        //2. 后序遍历最后一个元素是当前要切割的元素值
        int delimiterValue = postorder[postorderEnd-1];
        TreeNode* root = new TreeNode(delimiterValue);

        int delimiterIndex;
        //3. 在中序遍历中寻找切割点的位置
        for(delimiterIndex = 0; delimiterIndex < inorder.size(); delimiterIndex++)
        {
            if(inorder[delimiterIndex] == delimiterValue)
            {
                break;
            }
        }
        
        //4..切割中序数组
        //左中序数组(坚持左闭右开)
        int leftInorderStart = inorderStart;
        int leftInorderEnd = delimiterIndex;

        //右中序数组
        int rightInorderStart = delimiterIndex+1;
        int rightInorderEnd = inorderEnd;


        //5.切割后续数组
        //左后序数组(坚持左闭右开)
        int leftPostorderStart = postorderStart;
        //终止位置为postorder开始位置加上inorder的左中序数组的长度
        int leftPostorderEnd = postorderStart + (delimiterIndex - inorderStart);

        //右后序数组
        //左后序数组的终止位置，是左闭右开。所以这个位置也是右后序数组的开始位置
        int rightPostorderStart = postorderStart + (delimiterIndex - inorderStart);
        //最后一个元素已经处理过了
        int rightPostorderEnd = postorderEnd - 1;


        //6.递归处理左右区间
        
        root->left = build(inorder,leftInorderStart,leftInorderEnd,postorder,leftPostorderStart,leftPostorderEnd);
        root->right = build(inorder,rightInorderStart,rightInorderEnd,postorder,rightPostorderStart,rightPostorderEnd);

        return root;



    }
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if((inorder.size() == 0) || (postorder.size() == 0))
        {
            return NULL;
        }
        //左闭右开
        return build(inorder,0,inorder.size(),postorder,0,postorder.size());
       
    }
};
```

​	或者递归条件是叶子节点，那么需要在递归的时候判断，如果左右子树有为空的，那么需要设置NULL，如果不为空，才可以继续递归。

```C++
class Solution {
private:
    TreeNode* build(vector<int>& inorder, int inorderStart, int inorderEnd, vector<int>& postorder, int postorderStart, int postorderEnd)
    {
        //1. base case
        //如果postorder数组大小为0，说明里面是空节点了。
        //if (postorderStart == postorderEnd || inorderStart == inorderEnd) return NULL;
       
        //2. 后序遍历最后一个元素是当前要切割的元素值
        int delimiterValue = postorder[postorderEnd-1];
        TreeNode* root = new TreeNode(delimiterValue);

        if(postorderEnd - postorderStart == 1) return root;

        int delimiterIndex;
        //3. 在中序遍历中寻找切割点的位置
        for(delimiterIndex = 0; delimiterIndex < inorder.size(); delimiterIndex++)
        {
            if(inorder[delimiterIndex] == delimiterValue)
            {
                break;
            }
        }
        
        //4..切割中序数组
        //左中序数组(坚持左闭右开)
        int leftInorderStart = inorderStart;
        int leftInorderEnd = delimiterIndex;

        //右中序数组
        int rightInorderStart = delimiterIndex+1;
        int rightInorderEnd = inorderEnd;


        //5.切割后续数组
        //左后序数组(坚持左闭右开)
        int leftPostorderStart = postorderStart;
        //终止位置为postorder开始位置加上inorder的左中序数组的长度
        int leftPostorderEnd = postorderStart + (delimiterIndex - inorderStart);

        //右后序数组
        //左后序数组的终止位置，是左闭右开。所以这个位置也是右后序数组的开始位置
        int rightPostorderStart = postorderStart + (delimiterIndex - inorderStart);
        //最后一个元素已经处理过了
        int rightPostorderEnd = postorderEnd - 1;


        //6.递归处理左右区间
        if(leftPostorderEnd == leftPostorderStart || leftInorderEnd == leftInorderStart)
        {
            root->left = NULL;
        }
        else
        {
            root->left = build(inorder,leftInorderStart,leftInorderEnd,postorder,leftPostorderStart,leftPostorderEnd);
        }
        if(rightPostorderEnd == rightPostorderStart || rightInorderEnd == rightInorderStart)
        {
            root->right = NULL;
        }
        else
        {
            root->right = build(inorder,rightInorderStart,rightInorderEnd,postorder,rightPostorderStart,rightPostorderEnd);
        }
       
       

        return root;



    }
```

