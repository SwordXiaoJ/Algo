Day2 | LC203,LC707,LC206

链表基础

​	链表类型：

1. 单链表

 每一个节点就是有两部分，一部分是数据域，一部分是指针域。

2. 双链表

 每一个节点有三个部分，一部分是数据域，两个指针域，一个指向下一个节点，一个指向上一个节点。

 可以向前查询，也可以向后查询。

3. 循环链表

 首尾相连


 	

​	存储方式：

​		链表中的节点在内存中不是连续分布的，是随机分布在内存中的地址上。	

​	链表定义：
​		节点定义：			

```c++
struct ListNode{
    int val;
    ListNode* next;
    ListNode(int x): val(x),next(NULL){}
};
```

​	  链表操作：

  1. 删除节点

     将上一个节点指向被删除节点的下一个节点，然后释放被删除节点。

  2. 添加节点

     直接改变添加位置的前一个节点的指针，指向添加节点。

性能分析：
	

|      | 插入/删除（时间复杂度） | 查询（时间复杂度） | 使用场景                         |
| ---- | ----------------------- | ------------------ | -------------------------------- |
| 数组 | O(n)                    | O(1)               | 数据量固定，频繁查询，较少增删   |
| 链表 | O(1)                    | O(n)               | 数据量不固定，频繁增删，较少查询 |





LC203

​	题目链接 [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

​	解题思路 

1. 直接在原链表上操作：

   - 此时需要注意区分要删除的节点是头节点还是其他节点。因为头节点没有前一个节点，无法用general的方法完成删除操作。

   - 注意在删除头节点的过程中，需要使用while，而不是if。因为下一个新的头节点仍然有可能是待删除元素

   - 在删除一般节点的过程中，因为需要待删除节点的前一个节点。但是我们是单向链表，所以cur要从head开始，判断的是cur->next的值是否等于目标值。注意条件是cur != NULL && cur->next != NULL,因为我们需要操作cur->next,所以cur不能为空。因为我们要取cur->next的值，所以它也不能为空。
   
   - 也可以在general的操作中，先判断cur是不是为空，如果为空，直接返回null
   
2. 使用虚拟头节点

   - 统一了删除操作。

   - 注意在删除的过程中，循环仅仅需要判断cur->next 是否为NULL，不需要判断cur是否为NULL。因为cur是从dummy head开始的，dummy head是自己定义的，保证它不为空。在第一种方法中，head可能为空。

   - 在返回值的时候，不可以直接返回head。而是返回dummyHead->next,因为原来的head有可能已经被删除了。如果原来的head指向的第一个元素已经被删除了，此时就会报错。所以使用dummyHead->next更保险。



LC707

​	题目链接 [Design Linked List](https://leetcode.com/problems/design-linked-list/)

​	解题思路 

  1. 在C++中定义Node的时候，使用struct。写法如下，注意最后有一个;

     ```c++
      struct ListNode{
             int val;
             ListNode* next;
             ListNode(int val): val(val), next(NULL){}
         };
     ```

  2. get(int index)函数中，注意cur可以定义为dummyHead也可以定义为dummyHead->next,但是注意corner cases的情况，保证没有空指针。

  3. 构造单向链表的时候，可以采用dummyHead的方式，可以少处理很多的corner cases.





LC206

​	题目链接 [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

​	解题思路：

1. 双指针

  在遍历过程中，条件为cur != NULL，而不是cur->next != NULL，如果是cur->next != NULL，在最后一个节点就不会进行反转操作。

2. 递归

-  递归解法最好是在熟练掌握双指针写法的情况下，再去写。因为递归版本和双指针版本的代码逻辑是对应相同的。

-  递归关键是确定递归函数的输入和返回值。然后考虑清楚终止条件，最后再去考虑内部的逻辑。

-  双指针法的递归函数版本，递归函数完成的其实就是给两个节点，完成翻转操作。然后跳到下一组，继续完成翻转操作，直到全部翻转，从最后一层的prev开始，一层一层返回，直到最顶层。

3. 递归解法二

 从双指针的方法跳出来，从递归解法本身入手。

 构造一个 递归函数，它的功能是给一个head,可以完成翻转操作，并且返回新的链表的头节点。

 代码如下：

 ```C++
 ListNode* reverseList(ListNode* head) {
         if(head == NULL)
         {
             return NULL;
         }
         if(head->next == NULL) return head; //只有一个节点的时候
 
         ListNode* newhead = reverseList(head->next);
         head->next->next = head;
         head->next = NULL;
         return newhead;
          
      }
 ```


​           


​     

​		



