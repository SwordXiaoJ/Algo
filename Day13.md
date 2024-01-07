Day13 LC239,LC347





LC239

​	题目链接：[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

​	解题思路：

​	思路1：

​		看到最大值，最小值，一下子想到大顶堆/小顶堆。但是大小顶堆虽然可以维护最顶部元素是最大值/最小值，但是每次弹出的时候，也是这个元素。但是本题的滑动窗口，弹出的元素不一定是这个堆顶元素，而是随着滑动窗口移动，是变化的。

​		在看过官方解析以后，引入下标以后可以解决这个问题。不断添加新的元素到priority_queue中，然后判断此时的最大值可能并不在这个滑动窗口中，最大值可能已经从窗口左边被排出去了。如果是这种情况，这个最大值的下标肯定是在左边界的左面。那我们就一直弹出最大值，一直到找到在这个滑动窗口区间内的最大值就好。然后将找到的最大值添加到答案中。

​		

​	思路2：

​	 因此考虑自己构建一个队列来实现这个功能。（单调队列）。这个队列其实可以只维护有可能成为滑动窗口最大值的元素，并把它放在队首。**这个队列严格单调递减**

​	入队操作：每次入队的时候，先和队尾元素比较，如果比队尾元素小，直接放在对位。如果比队尾元素大，则它有可能成为最大值，就一直向前比较，不断从队尾弹出比较元素。直到找到合适的位置。

​	出队操作：判断移动窗口过程中丢失掉的元素是不是队首元素（最大值），如果不是，不用做任何操作，因为此时这个元素已经在之前就不在队列中了。如果是最大值，就正常从队头pop就可以了

​		



LC347

​	题目链接：[Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

​	解题思路：
  1. 利用map，统计数组中的元素和它们出现的次数。

  2. 进行排序。但是实际上我们不需要维护整个map的顺序，我们只需要利用大/小顶堆维护想要的前k个高频元素就可以了

     ​	难点：直观反应是使用大顶堆，但是大顶堆每次更新元素的时候，都要把最大的元素弹出，它没办法保留前K个高频元素。所以要使用小顶堆，每次把最小的元素弹出，最后小顶堆剩下的就是前k个最大元素

​	

​	总结：

  1. priority_queue定义方式

     默认容器是vector

     默认，大顶堆。小定堆通过std::greater来实现。

     ```C++
     std::priority_queue<int> maxHeap; //默认的排序方式将基于 operator< 对元素进行比较。这意味着队列的顶部（您通过 top() 访问的元素）将是所有元素中“最大”的元素。
     std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
     ```

     

     自定义比较方式的时候：

     ​	`std::priority_queue` 是一个类模板，它的比较方式是通过模板参数来指定的，这要求提供的是一个类型（如函数对象类型），而不是一个函数。必须是一个类型，std::priority_queue` 会在内部实例化这个类型的对象来进行元素比较。

     

     左>右 ，小顶堆

     右>左，大顶堆

     

```C++
		class mycomparison {
            public:
                bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
                    return lhs.second > rhs.second;
                }
    };
		//对频率排序（使用小顶堆) 自定义
        //priority_queue<Type, Container, Functional>;
        priority_queue<pair<int,int>,vector<pair<int,int>>,mycomparsion> que;
```



写快排的cmp函数的时候，`return left>right` 就是从大到小，`return left<right` 就是从小到大。

```C++
bool compare(int a, int b) {
    return a > b; // 降序排序
}

void quickSort(std::vector<int>& arr, int left, int right, bool (*comp)(int, int)) {
    if (left < right) {
        int pivot = arr[right];
        int i = left - 1;
        
        for (int j = left; j < right; j++) {
            if (comp(arr[j], pivot)) {
                i++;
                std::swap(arr[i], arr[j]);
            }
        }

        std::swap(arr[i + 1], arr[right]);
        int pi = i + 1;

        quickSort(arr, left, pi - 1, comp);
        quickSort(arr, pi + 1, right, comp);
    }
}
```





sort函数自定义排序方式(通过直接把比较函数作为参数传进去的方式，自定义排序方式)

```C++
bool compare(int a, int b) {
    return a < b;
}

int main() {
    std::vector<int> vec = {3, 1, 4, 1, 5, 9, 2, 6, 5};
    std::function<bool(int, int)> comp = compare;

    std::sort(vec.begin(), vec.end(), comp);

    for (int val : vec) {
        std::cout << val << " ";
    }
    return 0;
}
```



2.  温故复习遍历过程中的写法

   ```
   1. //尽量以后别用了，太长，容易写错
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

   



总结：

	1. C++中，添加元素的时候，push_back()和emplace()的区别

`push` 方法是用来向容器添加一个新元素的，这个新元素是通过传递给 `push` 方法的参数来构造的。在调用 `push` 时，会发生拷贝构造或移动构造（取决于参数类型），即使在容器内部创建元素之前，参数本身已经构造完毕了。

```C++
std::vector<MyClass> vec;
MyClass obj;
vec.push_back(obj);  // 调用拷贝构造函数
vec.push_back(MyClass()); // 调用移动构造函数
```

`emplace` 方法是 C++11 引入的，它允许在容器内部直接构造元素，从而避免了额外的拷贝或移动操作。`emplace` 方法接受的参数是用于构造新元素的构造函数的参数，而不是新元素本身。

```
std::vector<MyClass> vec;
vec.emplace_back(arg1, arg2, arg3); // arg1, arg2, arg3 是 MyClass 的构造函数参数
```

emplace()更高效

2. stack和queue的插入复杂度是O(1),deque的插入复杂度是O(logn)
