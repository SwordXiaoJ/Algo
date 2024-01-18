Day 24

​	回溯基础理论：

1. 回溯是递归的副产品，有递归就会有回溯。回溯本身并不高效。递归函数下面的部分就是回溯的逻辑

2. 一般可以解决以下问题：
       - 组合问题：N个数里面按照一定规律找出k个数的集合
       - 切割问题：一个字符串按照一定规则有多少种切割方式
       - 子集问题：一个N个数的集合里面有多少符合条件的子集
       - 排列问题：N个数按一定规律全排列，有几种排列方式
       - 棋盘问题：N皇后，解数独。
3. 组合无序，排列有序
4. 集合的大小构成树的宽度，递归的深度构成树的深度。
5. 回溯法模板：
       1. 回溯函数模板返回值和参数。返回值一般是void。回溯算法需要的参数不像是递归函数容易确定，所以一般先写逻辑，后确定参数。
       2. 终止条件
       3. 遍历过程。集合的大小构成树的宽度，递归的深度构成树的深度。

```
for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
    处理节点;
    backtracking(路径，选择列表); // 递归
    回溯，撤销处理结果
}
```

for循环就是遍历集合区间，可以理解一个节点有多少个孩子，这个for循环就执行多少次。

**for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历**



回溯法解决问题的核心就是抽象为树形结构（N叉树）



LC77

​	题目链接：[Combinations](https://leetcode.com/problems/combinations/)

​	解题思路：

​	总结：	

  1. startItem不是表示下标，因为选择范围是从1开始的，所以startItem其实表示开始的元素的值。

     ```C++
     class Solution {
     private:
         vector<vector<int>> result;
         vector<int> path;
         void backtracking(int n,int k,int startItem)
         {
             if(path.size() == k)
             {
                 result.push_back(path);
                 return;
             }
     
             for(int i = startItem; i <= n; i++)
             {
                 path.push_back(i);
                 backtracking(n, k,i+1);
                 path.pop_back();
             }
         }
     public:
         vector<vector<int>> combine(int n, int k) {
             backtracking(n, k,1);
             return result;
             
         }
     };
     ```

     

 2. 剪枝优化

    **可以剪枝的地方就在递归中每一层的for循环所选择的起始位置**。

    **如果for循环选择的起始位置之后的元素个数 已经不足 我们需要的元素个数了，那么就没有必要搜索了**。

- 已经选择的元素个数：path.size();
- 所需需要的元素个数为: k - path.size();
- 列表中剩余元素（n-i） >= 所需需要的元素个数（k - path.size()）
- 在集合n中至多要从该起始位置 : i <= n - (k - path.size()) + 1，开始遍历
- 这个地方的+1可以举个例子就可以了。
