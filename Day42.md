Day 42

​	0-1背包问题



​	有n件物品和一个最多能背重量为w的背包。第i件物品的重量是weight[i],得到的价值是value[i]，**每件物品只可以用一次**，求解哪些物品装进背包，价值最大。

​	暴力解法 其实是回溯，遍历每一件物品，他都有选和不选两个选择，所以可以使用回溯来做，只不过复杂度是O(2^n),指数级别的复杂度。

​	因此使用DP来做。

 1. dp [i] [j] 表示从下标为0-i的物品中任意取，放进容量为j的背包中，得到的最大价值为dp [i] [j].

 2. 推导公式：不放物品i，仍然是dp[i-1] [j] 。放物品i, dp[i-1] [j-weight[i] + value[i]

    dp[i] [j] = max(dp[i-1] [j] , dp[i-1] [j-weight[i] + value[i])

 3. 初始化：

    如果j为0的话，dp[i] [0] = 0;

    如果i为0的话，如果weight[i] > j, dp[0] [j] = 0;

    ​			  如果weight[i] <= j, dp[0] [j] = value[0];

    其实从递归公式： dp[i] [j] = max(dp[i - 1] [j], dp [i - 1] [j - weight[i]] + value[i]); 可以看出dp [i] [j] 是由左上方数值推导出来了，那么 其他下标初始为什么数值都可以，因为都会被覆盖。

    **初始-1，初始-2，初始100，都可以！**

    ```C++ 
    // 初始化 dp
    vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
    for (int j = weight[0]; j <= bagweight; j++) {
        dp[0][j] = value[0];
    }
    
    ```

    

 4. 确定遍历顺序：

    先遍历物品，还是先遍历书包都可以。因为从推导公式 可以看出，dp[i] [j] 是由左上方，和正上方推导出来的，哪种遍历都可以满足。但先遍历物品再遍历背包这个顺序更好理解。

    ```text
    // weight数组的大小 就是物品个数
    for(int i = 1; i < weight.size(); i++) { // 遍历物品
        for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
            if (j < weight[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
    
        }
    }
    
    // weight数组的大小 就是物品个数
    for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
        for(int i = 1; i < weight.size(); i++) { // 遍历物品
            if (j < weight[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
        }
    }
    ```

 5. 举例推导

    ```C++
    //二维dp数组实现
    #include <bits/stdc++.h>
    using namespace std;
    
    int n, bagweight;// bagweight代表行李箱空间
    void solve() {
        vector<int> weight(n, 0); // 存储每件物品所占空间
        vector<int> value(n, 0);  // 存储每件物品价值
        for(int i = 0; i < n; ++i) {
            cin >> weight[i];
        }
        for(int j = 0; j < n; ++j) {
            cin >> value[j];
        }
        // dp数组, dp[i][j]代表行李箱空间为j的情况下,从下标为[0, i]的物品里面任意取,能达到的最大价值
        vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
    
        // 初始化, 因为需要用到dp[i - 1]的值
        // j < weight[0]已在上方被初始化为0
        // j >= weight[0]的值就初始化为value[0]
        for (int j = weight[0]; j <= bagweight; j++) {
            dp[0][j] = value[0];
        }
    
        for(int i = 1; i < weight.size(); i++) { // 遍历科研物品
            for(int j = 0; j <= bagweight; j++) { // 遍历行李箱容量
                // 如果装不下这个物品,那么就继承dp[i - 1][j]的值
                if (j < weight[i]) dp[i][j] = dp[i - 1][j];
                // 如果能装下,就将值更新为 不装这个物品的最大值 和 装这个物品的最大值 中的 最大值
                // 装这个物品的最大值由容量为j - weight[i]的包任意放入序号为[0, i - 1]的最大值 + 该物品的价值构成
                else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
            }
        }
        cout << dp[weight.size() - 1][bagweight] << endl;
    }
    
    int main() {
        while(cin >> n >> bagweight) {
            solve();
        }
        return 0;
    }
    
    ```

    

​	

​	0-1 背包优化（滚动数组）

​	

​	二维降一维

​		dp[i] [j] = max(dp[i-1] [j] , dp[i-1] [j-weight[i] + value[i])

​		如果可以把dp[i-1]这一层拷贝到dp[i]上，那么推导公式为 dp[i] [j] = max(dp [i] [j], dp[i] [j-weight[i]] + value[i])

​		**与其把dp[i - 1]这一层拷贝到dp[i]上，不如只用一个一维数组了**，只用dp[j]（一维数组，也可以理解是一个滚动数组）。需要满足的条件是上一层可以重复利用，直接拷贝到当前层。

​		状态压缩技术的关键思想是观察到在填充 `dp` 表时，`dp[i][j]` 的值只依赖于上一行的值（即 `dp[i - 1][...]`）。这意味着，为了计算当前行的值，我们只需要知道上一行的值，而不是整个表的所有之前的值。

​	因此，如果我们能够在不丢失必要信息的情况下重用相同的一维数组来代替二维数组，那么我们就可以实现空间上的优化。这就是“拷贝到 `dp[i]`”的含义，它实际上意味着我们不再区分 `dp[i]` 和 `dp[i-1]`，而是使用单一的一维数组 `dp[j]` 来代替，其中 `j` 代表背包容量。



   1. 确定dp数组定义

      在一维dp数组中，dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]。

   2. 推导公式

      dp[j] 表示不放入物品i, dp[j - weight[i]] + value[i]) 表示放入物品i

      ```C++
      dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
      ```

   3. 初始化

      dp[0] = 0;

      其他的非0下标都初始化为0，只有这样才能保证每次最大值是有效的，而不是被初始化值覆盖。

   4. 遍历顺序

      **先遍历物品，再遍历背包。**

      ```C++
      for(int i = 0; i < weight.size(); i++) { // 遍历物品
          for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
              dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
      
          }
      }
      ```

      注意背包是**倒序遍历**的，因为倒序可以保证每个物品i只被放入一次。逆序确保在更新当前容量 `j` 的最大价值 `dp[j]` 时，使用的是不包括当前物品的前一个状态。这样可以避免一个物品被重复计算。

      **如果正序，前面的值已经更新为第i行的值，但是我们要求的dp[j]需要使用的是i-1行的状态。而从后往前则不会有这个问题。**

      

      不能先遍历背包，再遍历物品。因为遍历背包要求是倒序，如果先遍历背包，dp[j]就只会放入一个物品，即：背包里只放入了一个物品。

      ```C++
      void test_1_wei_bag_problem() {
          vector<int> weight = {1, 3, 4};
          vector<int> value = {15, 20, 30};
          int bagWeight = 4;
      
          // 初始化
          vector<int> dp(bagWeight + 1, 0);
          for(int i = 0; i < weight.size(); i++) { // 遍历物品
              for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
                  dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
              }
          }
          cout << dp[bagWeight] << endl;
      }
      
      int main() {
          test_1_wei_bag_problem();
      }
      
      ```

LC416

​	题目链接：[Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/

​	解题思路：

1. 抽象为背包问题，背包的容量为 sum / 2; 放入的物品重量和价值都是数字值； 每个元素不可以重复放入；  如果背包正好装满，那么久说明找到了满足条件的数组划分情况

  2. 背包容量j为能背的最大重量。本题中物品的重量和价值都是数字本身的值。

     dp[j] 为 容量为j的背包，装入物品以后，能得到的价值（重量）。如果得到的重量就是背包容量，那么背包就装满了

​		

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        //抽象为背包问题，背包的容量为 sum / 2; 放入的物品重量和价值都是数字值； 每个元素不可以重复放入；  如果背包正好装满，那么久说明找到了满足条件的数组划分情况
        //dp数组定义
        //dp[j] ，表示 背包总容量（最多的总重量）是j，放进物品后，得到的重量为dp[j]。如果得到的重量就是背包容量，那么背包就装满了
        vector<int> dp(10001,0);

        //递推公式
        //dp[j] = max(dp[j],dp[j-nums[i]] + nums[i]);
        int sum = 0;
        for(int i = 0; i  < nums.size(); i++)
        {
            sum += nums[i];
        }
        
       
        if(sum % 2 == 1) return false;
        int target = sum / 2;

        //初始化
        dp[0] = 0;
        for(int i = 0; i < nums.size(); i++)
        {
            for(int j = target; j >= nums[i]; j--) 
            {
                dp[j] = max(dp[j],dp[j-nums[i]] + nums[i]);
            }
        }

        if(dp[target] == target) return true;
        return false;
        
    }
}
```

​	总结：

		1. 如果题目给的价值都是正整数那么非0下标都初始化为0就可以了，如果题目给的价值有负数，那么非0下标就要初始化为负无穷。
		1. **注意在使用一维数组解决背包问题的时候，如果是倒序遍历背包的时候，要不使用类似判断条件 j >= nums[i] 来控制循环范围。如果选择遍历到0，则需要加判断条件，if( j > nums[i]) 才可以写递推公式**





思路2： 采用bool值的二维dp数组，这个更好理解一些

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {

        //dp数组定义
        //dp[i][j]表示从0-i为下标的元素中选择元素，装容量为j的背包，是否可以装得满。它的值为true 或者 false
        int sum = 0;
        for(int i = 0; i < nums.size(); i++)
        {
            sum += nums[i];
        }
        if(sum % 2 == 1) return false;
        int target = sum / 2;
        
        vector<vector<int>> dp(nums.size(),vector<int>(target+1,false));

        //递推公式,如果选择放入nums[i],则需要去判断dp[i-1][j-nums[i]]。如果选择不放入nums[i],则需要去判断dp[i-1][j]
        //dp[i][j] = dp[i-1][j] | dp[i-1][j-nums[i]]

        for(int i = 0; i < nums.size(); i++)
        {
            dp[i][0] = true;
        }
        for(int j = 0; j <= target; j++)
        {
            if(j == nums[0])
            {
                dp[0][j] = true;
            }
                
        }
        
        for(int i = 1; i < nums.size(); i++)
        {
            for(int j = 1; j <= target; j++)
            {
                if(j >= nums[i])
                {
                    dp[i][j] = dp[i-1][j] | dp[i-1][j-nums[i]];      
                }
                else
                {
                    dp[i][j] = dp[i-1][j];
                }
                
            }
        }



        return dp[nums.size() - 1][target];



        
    }
};
```

