Day17 LC110,LC257,LC404



LC110

​	题目链接：[Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

​	解题思路：

  1. 递归法

     ​	还是注意高度和深度的区分，高度是由下到上，后序遍历。深度是由上到下，前序遍历。

     ​	我在第一遍写的时候，没有注意到要求每一个节点都满足高度差小于等于1的要求。没有在leftHeight和rightHeight调用递归之后进行判断，导致花了一些时间来修正。

     ```C++
     class Solution {
     private:
         int getHeight(TreeNode* node)
         {
             if(node == NULL) return 0;
             int leftHeight = getHeight(node->left);
             if(leftHeight == -1) return -1;
             int rightHeight = getHeight(node->right);
             if(rightHeight == -1) return -1;
             if(abs(leftHeight-rightHeight) > 1)
             {
                 return -1;
             }
             else
             {
                 return 1+max(leftHeight,rightHeight);
             }
     
         }
     public:
         bool isBalanced(TreeNode* root) {
             if(root == NULL) return true;
             if(getHeight(root) == -1)
             {
                 return false;
             }
             else
             {
                 return true;
             }
     
             
         }
     };
     ```

     

     LC257

     ​	题目链接：[Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

     ​	解题思路：

     ​	(1) 回溯算法模板：	

     ```
     void backtracking(参数) {
         if (终止条件) {
             存放结果;
             return;
         }
     
         for (选择：本层集合中元素) {
             处理节点;
             backtracking(路径，选择列表); // 递归
             回溯，撤销处理结果
         }
     }
     
     ```

      

     1. 使用值传递完成回溯：在这个回溯函数中，使用的参数是string path，是值传递，不是引用传递。它仅仅做了内容拷贝，所以本层递归中，path + 该节点数值，但该层递归结束，上一层path的数值并不会受到任何影响(因为递归的下一层相当于只是借用了一下上一层的值来完成一个初始化，然后自己内部操作)。如果使用的string&  path,作为参数，那么引用传递，所以一定有path += to_string(cur->val);对应path.pop_back(),只不过如果这个数字是多位数，有多少位就要调用多少次pop_back()。

     ​       所以如果一定要用引用传递完成，可以考虑使用vector<int>来存储path.

     

     ​	使用值传递完成回溯：注意在传值的时候，使用path+"->"的形式，所以原来path不变化，不需要手动pop_back来回溯。如果是path = path + "->",就需要手动pop_back()

     ```C++
      void getAllPaths(vector<string>&result,TreeNode* cur,string path)
         {
             
             if(cur == NULL) return;
             path += to_string(cur->val);
             if(cur->left == NULL && cur->right == NULL)
             {
                 result.push_back(path);
                 return;
             }
             getAllPaths(result, cur->left,path+"->");
             getAllPaths(result, cur->right,path+"->");
     
         }
     ```

     使用引用传递完成回溯：

     ```C++
      void traversal(TreeNode* cur, vector<int>& path, vector<string>& result) {
             path.push_back(cur->val); 
             if (cur->left == NULL && cur->right == NULL) {
                 string sPath;
                 for (int i = 0; i < path.size() - 1; i++) {
                     sPath += to_string(path[i]);
                     if(i != path.size() - 1)
                     {
                         sPath += "->";
                     }
                 }
                 sPath += to_string(path[path.size() - 1]);
                 result.push_back(sPath);
                 return;
             }
             
             if(cur->left)
             {
                 traversal(cur->left, path, result);
                 path.pop_back(); // 回溯
             }
     
             if(cur->right)
             {
                 traversal(cur->right, path, result);
                 path.pop_back(); // 回溯
             }     
         }
     ```

     

总结：

	1. C++中string也可以pop_back();





LC404

​	题目链接：[Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)

​	解题思路：

  1. 非递归法，深度模板/广度模板，加一个遍历来统计结果。因为left leaves要通过它的父节点来判断，所以判断标准是遍历节点的左孩子不为空，但是左孩子的左右孩子都为空。

  2. 递归法（自己的方法更加好理解），使用一个global 变量去累加来统计结果。这样这个判断条件在递归中的位置更加合理。卡哥的代码是覆盖，我觉得不太好理解。

     ```C++
     class Solution {
     public:
         int result = 0;
         int sumOfLeftLeaves(TreeNode* root) {
             if(root == NULL ) return 0;
             if(root->left == NULL && root->right == NULL) return 0;
     
             if(root->left != NULL && root->left->left == NULL && root->left->right == NULL)
             {
                 result += root->left->val;
             }
             
             int leftValue = sumOfLeftLeaves(root->left);    // 左
            
             int rightValue = sumOfLeftLeaves(root->right);  // 右
     
            
             return result;        
         }
     };
     ```

     
