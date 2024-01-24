Day 30





LC332

​	题目链接：https://leetcode.com/problems/reconstruct-itinerary/description/

​	解题思路：

  1. 遍历ticket，统计每一个出发机场以及对应的到达机场，注意，一个出发机场对应多个到达机场，又因为是要求按照字典序排列，直观反应是使用字典。然后值使用set或者multiset。但是考虑一下，因为在遍历过程中，为了避免出现循环，所以在用了一条边以后，就要把这个删除掉。如果使用set作为值的话，删除以后由于set或者multiset是有序的，会导致删除点以后的所有元素重新排列，所以需要注意，在erase删除元素以后，更新iterator

     ```
     for (auto it = mySet.begin(); it != mySet.end(); /* no increment here */) {
             if (*it % 2 == 0) { // 条件：如果元素是偶数，则删除
                 it = mySet.erase(it); // 删除元素并更新迭代器
             } else {
                 ++it; // 只有当不删除元素时才递增迭代器
             }
         }
     ```

     注意这时候，使用item去遍历是不行的，会报错。因为erase以后，item的遍历会失效。

     ```
      for(auto& item : destination)
     ```

     

  2. 做了很久，都没有AC，说明如果插入和删除元素，会很麻烦。所以考虑采用map作为键值对中的值，在map中可以增加一个键值对，键表示目的地机场名称，值为一个数字，如果遍历过程中使用了，就--。回溯的过程中++，只有在这个数字大于0的情况下，表示这个目的地机场可以飞。**相当于说我不删，我就做一个标记！**如果“航班次数”大于零，说明目的地还可以飞，如果“航班次数”等于零说明目的地不能飞了，而不用对集合做删除元素或者增加元素的操作。

     

  3. 本题找到一条符合要求的路径就好，所以不需要遍历整棵树，所以设置返回值。如果需要遍历整棵树，则不设置返回值。

  4. **可以说本题既要找到一个对数据进行排序的容器，而且还要容易增删元素，迭代器还不能失效**



​	总结：

1. 在 C++ 中，当您使用 `operator[]` 访问 `std::unordered_map` 的一个元素时，如果该键（key）不存在,`unordered_map` 会自动插入一个新的键值对。新插入的键对应的值将被默认构造。

​	

```C++
class Solution {
private:
    unordered_map<string,map<string,int>> paths;
    vector<string> result;
    bool backtracking(int pathsNum)
    {
        if(result.size() == pathsNum)
        {
            return true;
        }

        map<string,int>& dest_map = paths[result[result.size()-1]];
        for(auto& item : dest_map)
        {
            if(item.second > 0)
            {
                result.push_back(item.first);
                item.second--;
                if(backtracking(pathsNum)) return true;;
                item.second++;
                result.pop_back();
            }
        }

        return false;



    }
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for(auto& item: tickets)
        {
            paths[item[0]][item[1]]++;
        }
        result.push_back("JFK");
        backtracking(tickets.size() + 1);
        return result;
        
        
    }
};
```





LC51

​	题目链接：[N-Queens](https://leetcode.com/problems/n-queens/)

​	解题思路：

 1. 二维矩阵中矩阵的高就是这棵树的高度（递归的深度，也就是有几行），矩阵的宽就是树形结构中每一个节点的宽度。（for循环的长度，也就是有几列）

 2. 在 `backtracking` 函数中，对于每一行，代码都是通过一个 `for` 循环来遍历所有列的。在这个循环中，它会尝试将皇后放置在当前行的不同列上，但对于任何给定的递归调用（即对于特定的行），皇后只会被放置在这一行的一个位置上。一旦在某一列成功放置了皇后并且通过了 `isValid` 的检查，它就会递归到下一行。这意味着同一行上不可能有多于一个皇后，因此不需要在同一行上进行检查。

    ```C++
    class Solution {
    private:
        vector<vector<string>> result;
        
        void backtracking(vector<string>& chessboard,int n,int curRow)
        {
            if(curRow == n)
            {
                result.push_back(chessboard);
                return;
            }
    
            for(int col = 0; col < n; col++)
            {
                if(isValid(curRow,col,chessboard,n))
                {
                    chessboard[curRow][col] = 'Q';
                    backtracking(chessboard, n,curRow+1);
                    chessboard[curRow][col] = '.';
                }
                
            }
    
        }
    
        bool isValid(int curRow,int col,vector<string>& chessboard,int n) // 不用检查行，因为每行都是依次放置的，行是进入isValid判断的条件，每次进入valid 判断，说明该行不可能再放入其他的。
        {
            for(int i = 0; i < n;i++) // 列检查
            {
                if(chessboard[i][col] == 'Q')
                {
                    return false;
                }
            }
            //左上方检查
    
            for(int i = curRow-1, j = col-1; i >= 0 && j >= 0; i--,j--)
            {
                if(chessboard[i][j] == 'Q')
                {
                    return false;
                }
            }
    
            //右上方检查
            for(int i = curRow-1, j = col+1; i >=0 && j < n; i--,j++)
            {
                if(chessboard[i][j] == 'Q')
                {
                    return false;
                }
    
            }
            return true;
        }
    public:
        vector<vector<string>> solveNQueens(int n) {
            vector<string> chessboard(n,string(n,'.'));
            backtracking(chessboard, n,0);
            return result;
    
    
    
        }
    };
    ```

    









​	总结：

  1. 数组先定义，后面初始化的方法

     
     在 C++ 中，您可以先定义一个 `std::vector<std::string>` 类型的数组，然后在程序的另一个部分初始化它。有几种方式可以做到这一点，下面是一些常见的方法：

     ### 定义并稍后初始化

     1. **定义**：首先在一个地方定义一个 `std::vector<std::string>`：

        ```
        cppCopy code
        std::vector<std::string> myVector;
        ```

     2. **初始化**：然后在程序的另一个地方初始化它：

        ```
        cppCopy code
        myVector = {"one", "two", "three"};
        ```

     ### 使用构造函数初始化

     如果您希望在定义时立即初始化向量，可以直接在定义时使用构造函数：

     ```
     std::vector<std::string> myVector = {"one", "two", "three"};
     ```

     或者：

     ```
     std::vector<std::string> myVector{"one", "two", "three"};
     ```

     ### 使用 `push_back` 添加元素

     您也可以逐个添加元素，特别是在初始化数据不是一开始就可用的情况下：

     ```
     std::vector<std::string> myVector;
     myVector.push_back("one");
     myVector.push_back("two");
     myVector.push_back("three");
     ```

  2. std::string(n, '.')可以创建一个字符串。





​	LC37

​		题目链接：[Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

​		解题思路：

1.  N皇后问题是因为每一行每一列只放一个皇后，只需要一层for循环遍历一行，递归来遍历列，然后一行一列确定皇后的唯一位置。

​	数独中棋盘的每一个位置都要放一个数字（而N皇后是一行只放一个皇后），并检查数字是否合法，解数独的树形结构要比N皇后更宽更深

  2. 本题只要找到一个有效解就好，所以设置一个返回值。在调用递归的时候，使用if(backtracking()) return true; 表示递归到最深处，只要找到一个有效解，就层层返回，一直到找到最终答案。

     

     由于本人还是比较熟悉有终止条件的回溯写法， 所以仍然加入了终止条件。

     终止条件就是遍历完所有的行，也就是row == board.size()的时候，为了在找到结果的时候不要去做不必要的回溯，所以在终止条件的时候，设置为返回true.

     值得注意的是如果在遍历过程中，col已经到达了最后一列，需要递归调用下一行，且从第一列开始。

     

     在遍历board的过程中，每次开始的i和j不是从0开始，是从递归传入的参数表示的位置开始。因此在判断时，如果这个位置已经被占用了，就递归调用下一层，而不是仅仅continue.因为考虑整个数独板的结构，递归地移动到下一个位置，这是 `continue` 无法实现的。如果这个位置没有被占用，则去从1-9的数字中依次填入，并判断合法性。如果合法，就填入，并递归下一个位置。

     如果9个数字都遍历完，还是没有合适的，就返回false.

     

     注意，为什么在位置没有被占用的时候，时if(backtracing) return true; 在被占用的时候，是直接return backtracking

     

     在位置没有被占用的时候，当 `backtracking(board, i, j + 1)` 被调用并返回 `true` 时，这意味着从当前位置开始的所有后续填充都找到了有效解，因此您返回 `true` 以表明找到了解决方案。如果 `backtracking` 对于所有尝试的数字都返回 `false`，那么您需要撤销当前位置的填充（即设置回 `'.'`），并返回 `false` 表明当前路径不可行。这里的关键是您在尝试不同的数字填充同一个位置，寻找可以使数独保持有效的那个数字。

     

     位置被占用的时候，使用 `return backtracking(board, i, j + 1);` 是因为您希望立即将结果（无论是 `true` 还是 `false`）返回给上一层递归。如果找到解决方案（返回 `true`），则整个递归链应该终止；如果返回 `false`，表明需要回溯。

     

     主要就是一个要在for循环中不断去尝试，一个就是希望立刻返回调用结果给上一层，不需要尝试。

     

     

     ```C++
     class Solution {
     bool backtracking(vector<vector<char>>& board,int row,int col)
     {
         if(col == board[0].size())
         {
             return backtracking(board,row+1,0);
         }
         if(row == board.size())
         {
             return true;
         }
     
         for(int i = row; i < board.size(); i++) // 行
         {
             for(int j = col; j < board[0].size(); j++) // 列
             {
                 if(board[i][j] != '.')
                 {
                     return backtracking(board,i,j+1);
                 }
                 for(char k = '1'; k <= '9'; k++)
                 {
                     if(isValid(i,j,k,board))
                     {
                         board[i][j] = k;
                         if(backtracking(board,i,j+1)) return true;
                         board[i][j] = '.';
                     }
                 }
                 return false;
     
             }
         }
         return true;
     }
     
     bool isValid(int row, int col, char val, vector<vector<char>>& board) {
         for (int i = 0; i < 9; i++) { // 判断行里是否重复
             if (board[row][i] == val) {
                 return false;
             }
         }
         for (int j = 0; j < 9; j++) { // 判断列里是否重复
             if (board[j][col] == val) {
                 return false;
             }
         }
         int startRow = (row / 3) * 3;
         int startCol = (col / 3) * 3;
         for (int i = startRow; i < startRow + 3; i++) { // 判断9方格里是否重复
             for (int j = startCol; j < startCol + 3; j++) {
                 if (board[i][j] == val ) {
                     return false;
                 }
             }
         }
         return true;
     }
     public:
         void solveSudoku(vector<vector<char>>& board) {
             backtracking(board,0,0);
         }
     };
     ```

     

     
