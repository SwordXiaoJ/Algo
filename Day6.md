Day4 | LC242,LC349,LC202,LC1



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

