Day 35

​	LC860

​	题目链接：[Lemonade Change](https://leetcode.com/problems/lemonade-change/)

​	解题思路：
​		简单模拟，一次AC

​		其中贪心的想法是在遇到20的时候，注意先使用美元10去找零，如果美元10不够找零，再使用美元5。因为美元5可以找零的情况多一些。





​	LC406

​	题目链接：[Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)

​	解题思路：

​		遇到有两个维度的问题的时候，都是先确定一个维度，然后去考虑另一个维度。不要同时考虑两个维度

​		优先按照身高先排列，或者优先按照k去排列。

​		如果按照k排列， 肯定是从小到大，k相同的时候，h小的在前面。但是这样排序以后，k和h都很乱。

​		如果按照身高排，肯定是从大到小。h相同的时候，k是从小到大排。这样确定了每个人前面的人都比它高。排列之后，去遍历，按照k表示的数字作为下标，插入到位置就可。这样因为后面遍历过程中，每次都保证前面已经遍历过的元素都比它大。所以我们不需要去考虑身高，只需要考虑他要满足的位置这一条件

​		

```C++
class Solution {
private:
    static bool cmp(vector<int>& a, vector<int>& b)
    {
        if(a[0] == b[0])
        {
            return a[1] < b[1];// 身高相同的情况下，k小的往前排
        }
        else
        {
            return a[0] > b[0];
        }
    }
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        vector<vector<int>> result;
        for(auto & item : people)
        {
            result.insert(result.begin()+item[1], item);
        }
        return result;
        
    }
};
```

​		

​		总结：

​			1. 使用vector是非常费时的，C++中vector（可以理解是一个动态数组，底层是普通数组实现的）如果插入元素大于预先普通数组大小，vector底部会有一个扩容的操作，即申请两倍于原先普通数组的大小，然后把数据拷贝到另一个更大的数组上。

所以使用vector（动态数组）来insert，是费时的，插入再拷贝的话，单纯一个插入的操作就是O(n^2)了，甚至可能拷贝好几次，就不止O(n^2)了。

   2. 可以考虑使用链表来完成。最后转为vector,这样性能好一些。

      ```C++
      vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
              sort(people.begin(),people.end(),cmp);
              list<vector<int>> result;
              for(auto & item : people)
              {
                  list<vector<int>>::iterator it = result.begin();
                  int position = item[1];
                  while(position--)
                  {
                      it++;
                  }
                  result.insert(it,item);
              }
              return vector<vector<int>>(result.begin(),result.end());
              
          }
      ```

   3. vector底层扩容的过程：

      当insert数据的时候，如果已经大于capicity，capicity会成倍扩容（重新申请一个二倍于原数组大小的数组，然后把数据都拷贝过去，并释放原数组内存），但对外暴漏的size其实仅仅是+1。底层数组的内存起始地址也会变。所以如果使用vector来做insert操作，表面是O(n^2)复杂度，但是底层可能做了很多次的全拷贝，所以复杂度是O(n^2+ t * n) ，t是底层拷贝的次数。





LC452

​	题目链接：[Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

​	解题思路：

​		贪心思路： 当气球出现重叠的时候，一起射。

​		result默认为1，因为至少有1支箭。

​		如果不重叠，那么result++;

​		如果重叠了，将i+1的右边界更新为i和i+1中的小值，然后进行下一次判断。更新为小值的原因是只有这样，才能保证后面的比较是合理的，因为后面比较是要和小的比较，如果还是重叠，它们就用一支箭。要取最小值才可以满足这个条件。

```C++
class Solution {
private:
    static bool cmp(vector<int>& a, vector<int> &b)
    {
        return a[0] < b[0];
    }
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if(points.size() == 0) return 0;
        sort(points.begin(),points.end(),cmp);
        int result = 1;
        for(int i = 0; i < points.size()-1; i++)
        {
            if(points[i][1] < points[i+1][0]) // 无重叠
            {
                result++;  
            }
            else // 有重叠,更新i+1的右边界，注意更新为i+1和i中的较小的值。
            {
                points[i+1][1] = min(points[i+1][1],points[i][1]); 
            }

        }
        return result;
        
    }
};
```

