Day4 | LC24,LC19,LC160,LC142



LC24

​	题目链接 [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

​	解题思路 

​		使用dummyHead来保证操作的general,但是在解题过程中一定要画图，可以方便理解，保证指针操作的合理性。

​		最终的方法虽然和示例有所不同，但是仍然一次性AC。

​	总结：

		1. 每次要操作翻转的一对节点的时候，cur一定要指向一对节点中之前的那一个节点，才可以操作两个节点的交换。
		1. **如果个数为偶数，cur->next为NULL为退出遍历的条件；如果个数为奇数，cur->next->next 为 NULL为退出遍历的条件。**





LC19

​	题目链接 [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

​	解题思路

​		使用快慢指针，但是因为删除操作中，需要直到待删除节点的前一个节点。所以需要slow多落后fast一步，所以fast先走n+1步。

​	总结：

​		直接AC了

​		





LC160

​	题目链接 [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

​	解题思路：

  1. 哈希表，直接存储A的所有元素。然后遍历B的元素，检查哈希表中是否有。如果有，返回。

     ```C++
     ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
             unordered_set<ListNode*> itemA;
             ListNode* curA = headA;
             while(curA != NULL)
             {
                 itemA.insert(curA);
                 curA = curA->next;
             }
             ListNode* curB = headB;
             while(curB != NULL)
             {
                 if(itemA.find(curB) != NULL)
                 {
                     return curB;
                 }
                 curB = curB->next;
             }
             return NULL;
             
         }
     ```

     

  2. 从题解中看到的（双指针）

     如果两个链表相交，那么将长短链表的尾部对齐。然后从短链表所对应的长链表的位置开始，二者同时移动，一直到结尾。如果有一样的值，就是相交。

     既然是相交节点，相交节点的后续部分一定完全一致，毕竟相交节点对于两个列表来说就是同一个节点，那么也就是说相交节点以及后续部分一定是位于列表最后部分的。所以只需要把长列表的起点使得该起点到结尾的距离与短列表开头到结尾开头的距离一致，之后同步向后移，只要有相交节点那么必然能同时遇到。

     但是注意需要先遍历获得A，B的长度。然后还要根据长度不同进行区分，来移动长的那一条链表。





LC142 

​	题目链接 [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

​	解题思路：

  1. 使用一个哈希表来遍历，将遍历到的节点先进行检查，再加入到unordered_set中。 注意，unordered_set中插入使用的是insert,检查元素是否存在可以有两种方式，find()和count()。如果使用count,复杂度为O(1)  如果使用find，复杂度为O(logn)。 count的返回值为1或者0，去检测这个元素有没有，因为unordered_set储存非重复元素。 find则返回它的Iterator，如果找到的话，找不到就是nodeset.end()

  2. 双指针法

     (1) 判断是否有环？ 使用双指针。

     因为fast是走两步，slow是走一步，**其实相对于slow来说，fast是一个节点一个节点的靠近slow的**，所以fast一定可以和slow重合。 如果快指针一次走三步，相对于满指针走两步。则有可能跳过慢指针。

     (2) 找到环的入口处

     ![image](https://github.com/SwordXiaoJ/Algo/blob/main/images/Linked%20List%20Cycle%20II.png)

     ​	所以在相遇时，slow = x + y.  fast = x + y + n(y + z) 表示fast已经在环中走了n圈

     ​	如图，我们可以得到以下等式：
     
     ​		
     $$
     2(x+y) = x + y + n(y+z)
     $$
        
     $$
     x = (n-1)(y+z) + z
     $$
      
     
     因为时间相同，fast的速度是slow的两倍，所以最后的距离也是slow的两倍。
     
     此处可以观察到n 一定 是大于等于1的
     
     如果n = 1，可以得到 x= z .
     
     这就意味着，**从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**。
     
     也就是在相遇节点处，定义一个指针index1，在头结点处定一个指针index2。
     
     让index1和index2同时移动，每次移动一个节点， 那么他们相遇的地方就是 环形入口的节点。
     
     
     
     n>1的时候，就是fast指针在环形转n圈之后才遇到 slow指针。一样可以通过这个方法找到 环形的入口节点，只不过，index1 指针在环里 多转了(n-1)圈。
     
     
     
     Notes:
     
     为什么第一次在环中相遇的时候，slow的步数一定是x+y而不是x+y+n个环的长度
     
     
     
     
