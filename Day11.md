Day11 | LC20,LC1047,LC150





LC20

​	题目链接：[Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

​	解题思路：

​		因为先看到的括号最后才匹配，后进入的括号先匹配。所以符合先进后出的规律，所以使用stack来解决这一问题。

​		其实就是分为三种情况解决，左括号多出来了，右括号多出来了，括号类型不相同。

​		如果栈中放的是左括号，那么每次看到右括号，还要判断一下左括号对应的右括号是啥。需要使用map来存储pair。

​		所以可以采取的改进是每次遇到一个左括号，就放入一个对应的右括号。这样在遍历比较的时候，就直接去比较就可以了。



​	总结：

  1. map初始化方法

     ```C++
     std::map<int, std::string> myMap;
     myMap.insert(std::make_pair(1, "one"));
     myMap.insert(std::make_pair(2, "two"));
     myMap.insert(std::make_pair(3, "three"));
     
     
     std::map<int, std::string> myMap;
     myMap.insert({1, "one"});
     myMap.insert({2, "two"});
     myMap.insert({3, "three"});
     
     std::map<int, std::string> myMap;
     myMap.insert(pair<int,std::string>(1,"one"));
     ```

     

  2.  使用switch case,必须加break

     ```C++
     switch(item)
                 {
                     case '(' : 
                         break;
                     case '[' : 
                         break;
                     case '{' : 
                         break;
                     case ')' : 
                         break;
                     case ']' : 
                         break;
                     case '}' : 
                         break;
                 }
     ```

     

     



LC1047

​	题目链接：[Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

​	解题思路：

​		使用栈直接解决，如果栈为空就直接入栈，如果不为空，判断下栈顶元素是否和待入栈元素相同，如果相同。它们是相邻的，弹出栈顶元素进行消除。如果不同，待入栈元素直接入栈。

​		一次AC

​		

总结：

1. 一些涉及到消除的操作，可以试着考虑能不能使用栈来解决问题。
1. reverse()函数，参数是迭代器，所以可以反转数组，也可以翻转字符串。注意，第二个参数迭代器所指向的位置，是要翻转的最后一个字符的后一位。reverse(str.begin(),str.end()); str.begin()是指的字符串的第一个字符。
1. **递归的实现就是：每一次递归调用都会把函数的局部变量、参数值和返回地址等压入调用栈中**，然后递归返回的时候，从栈顶弹出上一次递归的各项参数，所以这就是递归为什么可以返回上一层位置的原因。





LC150

​	题目链接：[Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

​	解题思路：

​		使用栈来完成这道题，遇到数字的时候，将数字入栈。遇到运算符号的时候，从栈顶取出两个元素进行运算，将运算结果入栈。注意出栈的顺序和入栈是相反的，因此计算的时候，第一个出栈的元素位于运算的后位，第二个出栈的元素位于运算的前位。这对于减法和除法，会造成影响。



​		

​	总结：

		1. str to long long, stoll(string);
		1. str to int, stoi(string)

​		

