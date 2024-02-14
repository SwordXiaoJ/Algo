Day 50

​		Kama 57

​			题目链接：Kama57 [https://kamacoder.com/problempage.php?pid=1067

​			解题思路：

​				此题改为每次可以爬1-m级台阶，其实就变成了背包问题。也就是把背包装满有多少种办法。物品就是每次可以爬的台阶（m）。1阶，2阶，.... m阶就是物品，楼顶就是背包。每一阶可以重复使用，例如跳了1阶，还可以继续跳1阶。所以是完全背包问题。本题其实就转换为了求排列的问题。

​				排列问题先遍历背包，再遍历物品。

```C++
#include<iostream>
#include<vector>
using namespace std;
int main()
{
    int m,n; // m meas
    
    while(cin >> n >> m)
    {
        vector<int> dp(n+1,0);
        dp[0] = 1;
        for(int i = 0; i <= n; i++) //先遍历背包
        {
           for (int j = 1; j <= m; j++) { // 遍历物品
                if (i - j >= 0) dp[i] += dp[i - j];
            }
            
        }
        cout << dp[n] << endl;
        
    }
    
}
```

​		LC322

​			题目链接：https://leetcode.com/problems/coin-change/

​			一次AC，没什么难度，主要是从纯粹的完全背包问题变为了求最小值。所以在初始化的时候不能初始化为0，需要初始化为INT_MAX。 先遍历物品还是先遍历背包，对这道题来说，无所谓。不涉及到求排列/组合的问题。 但是因为初始化为最大值，所以在使用递归公式的时候，需要加判断。

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        //dp[j]表示凑总数量为j的amount，最少需要的coins数量为dp[j]
        vector<int> dp(amount+1,INT_MAX);

        //递推公式,不要i这个coin，和要i这个coin
        //dp[j] = min(dp[j],dp[j-i]+1)

        dp[0] = 0;

        for(int i = 0; i < coins.size(); i++)
        {
            for(int j = coins[i]; j <= amount; j++)
            {
                if(dp[j - coins[i]] == INT_MAX)
                {
                    continue;
                }
                dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
            }
        }
        if (dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```

​		LC279

​			题目链接：[Perfect Squares](https://leetcode.com/problems/perfect-squares/)

​			解题思路：
​				背包容量是n，物品是完美平方数。问题转换为装满背包容量为n的背包，所用的最少数量为多少。

​				从递归公式dp[j] = min(dp[j - i * i] + 1, dp[j]);中可以看出每次dp[j]都要选最小的，**所以非0下标的dp[j]一定要初始为最大值，这样dp[j]在递推的时候才不会被初始值覆盖**。



​				dp[0] = 0完全是为了递归公式可以进行下去

```C++
class Solution {
public:
    int numSquares(int n) {
        //dp[j]表示装满容量为j的背包，所用的最小数量的完美平方数为dp[j]

        vector<int> dp(n+1,INT_MAX);

        //递归公式,用这个完美平方数，以及不用这个完美平方数
        //dp[j] = min(dp[j], dp[j-i*i] + 1)


        //初始化
        dp[0] = 0;
     
        for(int i = 1; i * i <= n; i++)
        {
            for(int j = i*i; j <= n; j++)
            {

                dp[j] = min(dp[j], dp[j-i*i] + 1);
            }
        }

        return dp[n];
        
    }
};
```

​			
