Day6 | LC242,LC349,LC202,LC1



哈希表基础（本质就是一个数组+哈希函数） 

​	哈希表：通过关键码的值而直接进行访问，主要用来**快速判断一个元素是否出现在集合里面**。哈希表使用哈希函数将键（key）转换为数组的索引。哈希表内部通常是一个数组，用来存储数据。

​	在 `std::unordered_set` 这种数据结构中，它本质上是一个哈希表的特例，但只存储键（Key），而没有与之对应的值（Value）。这意味着，在 `std::unordered_set` 中，集合的每个元素既是键也是值。

​	哈希表有三种，数组，set，map

​	set有三种

​	

| 类型               | 底层   | 是否有序 | 数值是否可以重复 | 数值是否可以更改 | 查询效率 | 增删效率 |
| ------------------ | ------ | -------- | ---------------- | ---------------- | -------- | -------- |
| std::set           | 红黑树 | 有       | 否               | 否               | O(log n) | O(log n) |
| std::multiset      | 红黑树 | 有       | 是               | 否               | O(log n) | O(log n) |
| std::unordered_set | 哈希表 | 无       | 否               | 否               | O(1)     | O(1)     |

 	map有三种

​	

| 类型               | 底层   | 是否有序 | 数值是否可以重复 | 数值是否可以更改 | 查询效率 | 增删效率 |
| ------------------ | ------ | -------- | ---------------- | ---------------- | -------- | -------- |
| std::map           | 红黑树 | key有    | key否            | key否            | O(log n) | O(log n) |
| std::multimap      | 红黑树 | key有    | key是            | key否            | O(log n) | O(log n) |
| std::unordered_map | 哈希表 | key无    | key否            | key否            | O(1)     | O(1)     |







LC242

​	题目链接 [Valid Anagram](https://leetcode.com/problems/valid-anagram/)

​	解题思路：

​		题目就是就是比较两个字符串中字符以及它们出现的次数。所以需要一个数据结构来记录次数，set是不行的。可以使用map,但是map比较麻烦。比较简单的方法是使用一个26的数组表示26个字母，然后统计个数就好。

​		一次AC



LC349

​	题目链接[Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

​	解题思路：

​		我是直接通过两个unordered_set统计元素，然后进行比较，一次AC。但是其实可以利用unordered_set和vector之间的相互构造直接解决。

​	总结：

​		unordered_set也可以通过vector来构造

​		vector可以通过unordered_set来构造，vector<int>result(unoredered_set.begin(), unordered_set.end())

​		

LC202

​	题目链接 [Happy Number](https://leetcode.com/problems/happy-number/)

​	解题思路：

​		题目看到会陷入无限循环，且sum是1是判断是否是happy的标准，所以考虑使用哈希表存储每一次运算以后的sum。

​		如果遇到1，就说明是happy number。如果没有，就一直循环算，只不过要判断一下，新算出来的数之前有没有出现过，如果出现过，说明陷入无限循环了，返回false.

​		一次AC



LC1

​	题目链接 [Two Sum](https://leetcode.com/problems/two-sum/)

​	解题思路：

​		使用哈希表，但是因为题目要求返回的是下标，所以我们不仅需要判断一个元素是否满足条件，还需要记录它的下标。因此考虑使用map来完成任务，map的key是元素的值，map的value是它的下标。
​		一次AC



​	

总结：

​	1. 遍历方式：

​	   std::unordered_set<int> mySet = {1, 2, 3, 4, 5};

```C++
1. 
    for (std::unordered_set<int>::const_iterator it = mySet.cbegin(); it != mySet.cend(); ++it) {
    std::cout << *it << " ";
}

2. 
   for (const auto& element : mySet) {
    std::cout << element << " ";
}

3.
    for (auto it = mySet.begin(); it != mySet.end(); ++it) {
    std::cout << *it << " ";
}
```

​	std::map<int, std::string> myMap = {{1, "one"}, {2, "two"}, {3, "three"}};

```C++
1. 
for (std::map<int, std::string>::iterator it = myMap.begin(); it != myMap.end(); ++it) {
    std::cout << it->first << " => " << it->second << '\n';
}

2. 
  for (const auto& kv : myMap) {
    std::cout << kv.first << " => " << kv.second << '\n';
}

3.
    for (auto it = myMap.begin(); it != myMap.end(); ++it) {
    std::cout << it->first << " => " << it->second << '\n';
}
```

  	

2. find()对应的判断条件是set.find() != set.end()
2. Map的插入方式

```C++
numsMap.insert(pair<int,int>(nums[i],i));
```

