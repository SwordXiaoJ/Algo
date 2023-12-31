Day1 | LC704, LC 27 (LC34,LC35,LC69,LC367)、

数组理论基础：
	已经学习过基本知识，此处仅记录一些关键点：

1. C++中vector的底层是array

2. 数组中元素不能删除，只能覆盖

3. C++中二维数组的地址连续，Java因为不对程序员暴露元素地址，所以地址不连续。

   

LC704

​	题目链接 [Binary Search](https://leetcode.com/problems/binary-search/)

​	解题思路 

​		二分法（解决寻找元素问题）<u>**前提是数组有序**</u>

​		循环不变量：左闭右闭，左闭右开

​		左闭右闭：

```c++
left = 0;
right = numsize - 1;
while(left <= right) // 取决于现在的left == right 合不合法在这个区间中
{
    middle = (left + right) / 2;
    if(nums[middle] > target)
    {
        right = middle - 1; // middle 还是 middle - 1还是取决于区间定义，因为middle一定不是我们所需要的值，所以接下来的区间一定是不包括这个值，所以是middle  - 1     
    }
    else if(nums[middle] < target)
    {
        left = middle + 1;
    }
    else
    {
        return nums[middle];
    }
}
return -1;
```

​		左闭右开：

```c++
left = 0;
right = numsize;
while(left < right) // left = right 不合法
{
    middle = (left + right) / 2;
    if(nums[middle] > target)
    {
        right = middle ; // middle 还是 middle - 1还是取决于区间定义，因为middle一定不是我们所需要的值，但是右边是开的，所以right = middle, 因为不包含这个right 
    }
    else if(nums[middle] < target)
    {
        left = middle + 1;
    }
    else
    {
        return nums[middle];
    }
}
return -1;
```

​	问题

​		注意返回值的要求

​	总结

​		此题为二刷，主要是再次回顾关于二分法的关键点。主要是要根据区间的定义合理的处理边界条件。要区分清楚左闭右闭区间和左闭右开区间。



LC27

​	题目链接 [Remove Element](https://leetcode.com/problems/remove-element/)

​	解题思路 

 1. 暴力解法（O(n^2))

    用两个for循环

2. 双指针法（快慢指针）

   核心思路是用一层for循环完成两层for循环要做的事

   **快指针是寻找新数组所需要的元素**

   **满指针是新数组的下标值（要更新的位置）**

```c++
slow = 0;
for(int fast = 0; fast < nums.size(); fast++)
{
    if(nums[fast] != target)
    {
        nums[slow] = nums[fast];
        slow++;
    }
   
}
return slow; //slow是新数组的大小
```

​	问题

1. 注意在暴力解法中，要提前获取到数组的长度size,遍历过程中使用size作为条件，而不是使用nums.size(), 尤其是第一层循环，如果使用nums.size(),会超时。  
2. 使用双指针的时候，判断条件一定是不等于，等于的话，比较难写。因为等于的情况下，判断语句内什么都不需要执行。

​	总结

		1. vector中的erase函数是一个O(n) 的操作，主要参数是iterator，来指定删除的元素。注意std::remove并不是删除元素，因为size()没有变化，仅仅做了元素的替换而已。
		1. 看似双指针法没有删除的操作，但是其实遇到要删除的元素之后，满指针就不动，快指针移动一位，然后进行覆盖操作。其实就实现了删除的操作。

​		

Additional:

LC35:

​	[Search Insert Position](https://leetcode.com/problems/search-insert-position/)

​	看到有序数组，可以考虑使用二分法



LC34:

​	[Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

1. **return的结果是vector<int>的时候，可以这么构造返回结果**

​	   return {1,2}  或者 return vector<int>{1,2}

​           这样比vector<int> result;  result.push_back(1); 速度快而且占用空间小。\

​	   也可以vector<int> result(2,0); result[0] = 1; result[1] = 2;

   2. **vector的初始化方法**

      - vector<int> result; 空的
      - vector<int> result(10);  大小为10，默认值为0
      - vector<int> result(10,5);  大小为10，默认值为5
      - vector<int> result = {1,2,3,4,5}; C++11特性

   3. 本题主要还是利用二分法，主要注意的是因为不具有唯一性，所以需要考虑你需要的是左边界还是右边界。如果需要左边界，需要在**相等的情况下持续收缩右边界**，使得寻找的值无限逼近于左边界。反之亦然。

      

      

LC69:

​	[Sqrt(x)](https://leetcode.com/problems/sqrtx/description/)

 	1. 这道题和LC367类似，但是注意他需要输出最后的最近的整数值。所以在小于的情况下，要去判断下一个整数值是否是大于target的，如果是，直接存储答案并返回。如果不是，再update左边界。



LC367:

​	[Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/)

 1. 第一眼看过去不像是二分查找，因为不存在要找的target。但是仔细考虑，要找的target就是target ^ 2 <= num的最大值。

    

 2. 暴力解法就是遍历，但是注意**i * i ，当i是INT_MAX的时候，会溢出，所以需要改为(long long) i * i.** 但是时间会超时







​       



