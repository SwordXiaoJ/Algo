Day10 | LC232,LC225



栈与队列基本知识

​	stack: 先进后出

​	queue:先进先出

​	栈和堆是STL（C++标准库）里面的的数据结构。我们使用的是SGI STL 里面的数据结构。

​	栈： 提供push和pop接口，不提供迭代器。不像是set 或者map 提供迭代器iterator来遍历所有元素。**栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能。vector,deque,list都可以）。**所以STL中栈往往不被归类为容器，而被归类为container adapter（容器适配器）。默认是deque为底层实现。



​	队列：SGI中默认也是deque为底层，不允许有遍历行为。



```cpp
std::stack<int, std::vector<int> > third;  // 使用vector为底层容器的栈
```

```cpp
std::queue<int, std::list<int>> third; // 定义以list为底层容器的队列
```

​	

std::deque	

`std::deque`（双端队列）是 C++ 标准模板库（STL）中的一个容器类，它允许在容器的两端高效地插入和删除元素。`std::deque` 是“double-ended queue”的缩写，提供了类似于 `std::vector` 的功能，但与 `std::vector` 相比，它在两端进行操作时更高效。

1. **双端操作**：`std::deque` 支持在队列的前端和后端添加（`push_front`, `push_back`）和删除（`pop_front`, `pop_back`）元素。
2. **随机访问**：像 `std::vector` 一样，`std::deque` 也支持随机访问，这意味着你可以通过索引直接访问任何元素。
3. **动态大小**：`std::deque` 的大小可以根据需要动态增长或缩减。deque和vector需要resize来调整大小。
4. **非连续存储**：与 `std::vector` 不同，`std::deque` 不保证所有元素都存储在连续的内存块中。它通常是由多个较小的内存块组成的，这使得在两端添加或删除元素时不需要移动所有现有元素。





LC232

​	题目链接：[Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

​	解题思路：
​		使用两个stack来完成这个工作，加入元素和empty都很简单。pop的时候，注意要在stkOut中的元素为空的时候，再把stkIn中的所有元素加入到stkOut中。而且peek函数和pop函数高度相似，可以复用pop函数。

​		一次AC



LC225

​	题目链接：[Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)

​	解题思路：

​		一次AC

​	总结：

​		其他函数都是简单。在top()函数的时候，在最后将队尾元素放到队前并返回以后，注意还要把它复原回去。

