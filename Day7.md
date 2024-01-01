Day7 | LC454,LC383,LC15,LC18



LC454

​	题目链接 [4Sum II](https://leetcode.com/problems/4sum-ii/)

​	解题思路：

​	一次性AC

​	暴力解法就是四个循环。从而想到可以先遍历前两个数组，存储它们的不同组合和以及出现的次数（使用Map）。然后再遍历后两个数组，寻找target-c-d是否存在于哈希表中。

​	总结：

  1. Map和vector可以使用[]访问元素，set不行。

  2. 遍历vector也可以使用**(const) auto &**;  

     ```
     vector<int> result;
     for(auto& item: result)
     ```

3. 再遍历后两个数组，统计结果的时候。注意找到符合条件的元素时，result += map中符合条件的二元组的个数，而不是直接加一。



LC383

​	题目链接 [Ransom Note](https://leetcode.com/problems/ransom-note/)

​	解题思路：

​		一次性AC

​		之前做过那道字符串的题，明白字符串可以使用vector存储作为哈希表，又因为magazine是参照表，所以统计它的字符个数。然后遍历ransomNode即可。



LC15

​	题目链接 [3Sum](https://leetcode.com/problems/3sum/)

​	解题思路：
​		使用类似两数之和的哈希表来做的话，去重的逻辑比较复杂。因此引入了新的方法，之前学习过的双指针法，利用一个i来遍历所有元素,left为紧挨着遍历元素的下一个元素，right为末尾元素。



​	总结：

  1. **去重的时候使用nums[i] = nums[i-1]**，而不是nums[i] = nums[i+1],因为nums[i+1]实际上就是要判断的nums[left]，如果使用i+1进行判断，会导致去重变为三元组内部元素去重，而不是三元组之间去重。变成了要求三元组内部不允许有重复元素。

  2. 在边界判断的时候，一般代入判断，while(left < right)的时候，取考虑left == right 的时候，是不是我们需要的，发现在==的时候，left和right指向同一个值，此时无法构造三元组。所以判断条件不能有left == right.

  3. left和right的去重操作，如果放在while(left < right)的地方，会导致一开始就可能一直去重，漏掉一些情况，例如元素全部为0. 所以需要放在判断满足条件的地方，先放入第一组满足条件的三元组之后，再进行left和right的去重。

  4.  注意一些边界，我在使用nums[i] = nums[i-1]的时候，忘记了加上判断条件i > 0，花了很久来debug.

     



LC18

​	题目链接 [4Sum](https://leetcode.com/problems/4sum/)

​	解题思路：

​		仍然延续三数之和的思想，只不过需要加一层循环。

​	总结：

		1. 最外层剪枝的时候，注意此时我们设置了target，而不是0。target的值可以是负数，所以负数的情况下无法剪枝。只有正数或者零的情况下下才可以剪枝。
		1. **在第二层去重的时候，判断条件是nums[i] = nums[i-1],如果不加条件，又会变成nums[i]和nums[k]不能相等。仿照前面的去重，加入判断条件i > k + 1，相当于隔了一个元素以后开始判断去重条件。**
		1. **第二层剪枝可以不加，也可以通过测试。但是要加的话，就是依然使用target为正数和0两种情况，进行剪枝操作。只不过此时要换成break，来执行下一层操作。继续去寻找新的元素，而不是直接return.**
