Day8 | LC344,LC541,Cama 54, LC151,Cama55





LC344

​	题目链接：[Reverse String](https://leetcode.com/problems/reverse-string/)

​	解题思路：

​		本题第一反应是使用一个stack,直接存入元素，然后依次出栈即可。但是要求in-place with O(1) extra memory.

​		因此考虑双指针法，左右指针，依次向中间逼近就可。

​	

​	总结：

	1. reverse()函数用法，本题关键部分用库函数解决，所以不可直接用库函数解题

```C++
  std::vector<int> vec = {1, 2, 3, 4, 5};

    // 反转 vec 的元素
  std::reverse(vec.begin(), vec.end());
 //begin是包含在被反转的元素中，end()是不包含的
  std::reverse(vec.begin(),vec.end() + vec.size())

```

**begin()：返回指向容器中第一个元素的迭代器。**
**end()：返回指向容器中最后一个元素之后的位置的迭代器，这不是一个有效的元素位置，而是容器末尾的“过去端”（past-the-end）位置。**

2.  如果题目关键的部分直接用库函数就可以解决，建议不要使用库函数。

​	如果库函数仅是解题过程中的一小部分，并且你已经很清楚这个库函数的内部实现原理的话，可以考虑使用库函数。

​		



LC541

​	题目链接： [Reverse String II](https://leetcode.com/problems/reverse-string-ii/)

​	解题思路：

​		一次AC

​		本题，明显翻转字符串函数仅仅是题目的一部分，所以可以使用库函数。

​		思路为遍历字符串，每次的步长都是2k,表示数了2k个字符。然后根据题目要求判断剩余字符的个数。

​		注意在使用reverse()函数的时候，第一个参数是指向数组第一个元素的迭代器(vec.begin()),第二个函数是指向数组末尾元素后一位的迭代器(vec.end()  或者 vec.begin() + vec.size())，不是指向最后一个元素。



Cama54

​	题目链接： [替换数字（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1064)

​	解题思路：

​		一次AC

​		先遍历一遍字符串，统计出待替换数字的数量，这样可以扩充字符串的长度。

​		然后使用双指针，开始遍历扩充后的字符串。old指针遍历原字符串，遇到字符，就复制到new指针的位置；遇到数字，就进行替换。	

​	总结：

  1. 如果在扩充长度之后，从前往后填充，就是O(n^2)的算法了，因为每次添加元素都需要将添加元素之后的所有元素整体往后移动。所以是从后往前添加。

  2. 注意ACM模式下，最外面是一层大的循环来读取输入

     ```
     string s;
     while(cin >> s)
     {
     	//算法部分代码
     	cout << s << endl;
     }
     ```

     





LC151

​	题目链接：[Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

​	解题思路：

  1. 使用C++内置的istringstream，可以直接去除掉空格的影响。

     ```C++
      string reverseWords(string s) {
             std::vector<string> tokens;
             std::istringstream iss(s);
             std::string token;
             while(iss >> token)
             {
                 tokens.push_back(token);
             }
             string result;
             for(int i = tokens.size() - 1; i >= 0; i--)
             {       
                 result += tokens[i];
                 if(i != 0)
                 {
                     result += ' ';
                 }         
             }
             return result;     
         }
     ```

     

  2. 使用C++去实现一个split函数

     ```C++
     class Solution {
     public:
         string reverseWords(string s) {
             vector<string> stringVec;
             string token;
             for(char item : s)
             {
                 if(item != (char)' ')
                 {
                     token += item;      
                 }
                 else
                 {   
                     if(token != "")
                     {
                         stringVec.push_back(token);   
                         token = "";
                     }           
                 }
             }
             //读完以后，最后一个单词还未添加进去
              if(token != "")
             {
                 stringVec.push_back(token);       
                 token = "";
             }           
             string result;
             
             for(int i = stringVec.size() - 1; i >= 0; i--)
             {
                 result += stringVec[i];
                 if(i != 0) result += " ";
             }
             return result;   
         }
     };
     ```

  3. 使用题解中的思路

     - 移除多余空格

       - 这部分的代码，使用的是快慢指针，先实现了去除所有空格的操作，然后考虑，在这基础上保留一个空格。最后发现有一些情况下，会产生结尾多一个空格。判断一下，如果结尾是一个空格，则删除它。

     - 将整个字符串反转

     - 将每个单词反转

       ```C++
       class Solution {
       private:
           void removeExtraSpaces(string& s)
           {
               int fast = 0;
               int slow = 0;
       
               while(fast < s.size())
               {
                   if(s[fast] != ' ')
                   {        
                       //仅while这部分是去除所有空格
                       while(fast < s.size() && s[fast] != ' ')
                       {
                           s[slow] = s[fast];
                           slow++;
                           fast++;
                       }
                       //加上这个if以后，可以给每个单词后添加一个空格
                       if(fast != s.size() )
                       {
                           s[slow] = ' ';
                           slow++;
                       }   
                   }
                   else
                   {
                       fast++;
                   }          
               }
               // 如果最后一个字符是空格，则去除它
               if (slow > 0 && s[slow - 1] == ' ') {
                   slow--;
               } 
               s.resize(slow);    
           }
       
           void reverse(string& s,int left, int right)
           {    
               while(left <= right)
               {
                   swap(s[left],s[right]);
                   left++;
                   right--;
               }
           }
       
       public:
           string reverseWords(string s) {
               removeExtraSpaces(s);
               cout << s <<endl;
               reverse(s,0,s.size()-1);
       
               int wordStart = 0;
              
               for(int i = 0; i < s.size(); i++)
               {
                  
                   if(s[i] == ' ')
                   {
                       reverse(s, wordStart, i - 1);
                       wordStart = i+1;
       
                   }
                   if( i == s.size() - 1)
                   {
                       reverse(s, wordStart, i);
                   }
               }
       
               return s;
       
               
           }
       };
       ```

       

​	

​	总结：

  1. C++没有split函数，使用istringstream按空白字符（空格、制表符等）分割字符串，它会自动跳过

     ```c++
     std::istringstream iss(s);
     std::vector<std::string> words;
       while (iss >> word) {
             words.push_back(word);
         }
     ```

     `std::istringstream` 是 C++ 标准库中的一个类，它提供了从字符串中读取数据的功能。这个类属于 C++ 标准库中的输入流类（istream），具体来说是基于字符串的输入流。`std::istringstream` 继承自 `std::istream` 类，并且包装了一个 `std::string`，允许你像处理输入流（如 `std::cin`）一样处理字符串。

     `std::istringstream` 默认使用空白字符（如空格、制表符和换行符）作为分隔符来分割字符串。当你使用流提取操作符 `>>` 从 `std::istringstream` 读取数据时，它会自动忽略这些空白字符，并将连续的非空白字符序列作为一个独立的单元（如一个单词或数字）读取出来。(多个空格，开头结尾有空格都可以处理)

     ```C++
     	std::string str = "123 456 789";
         std::istringstream iss(str);
     
         int a, b, c;
         iss >> a >> b >> c;
     ```

     

     

     

     按照任意字符进行分割	

     ```C++
     #include <sstream>
     #include <vector>
     #include <string>
     
     std::vector<std::string> split(const std::string& s, char delimiter) {
         std::vector<std::string> tokens;
         std::stringstream ss(s);
         std::string token;
     
         while (std::getline(ss, token, delimiter)) {
             if (!token.empty()) {
                 tokens.push_back(token);
             }
         }
     
         return tokens;
     }
     ```

     **`token`**：这是一个 `std::string` 对象，用于存储从 `ss` 中读取的每一段字符串。每次调用 `std::getline` 时，它都会被填充为直到下一个分隔符之前的字符串。

     

     std::getline函数

     ```
     std::getline(std::cin, str, delimiter);
     ```

     

  2.  for(auto a : vec) 和 for(auto& a : vec)的区别，加了&表示引用，所以在循环中要修改这个a的时候，加上&。如果仅仅是访问，可以不加

  3. string中有clear函数，string.clear();

​	



Cama55

​	题目链接：[右旋字符串（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1065)

​	解题思路：

		1.  用substr()函数
  		2.  采用题解的方法
        		1.  先旋转后面k个字符。
        		2.  再反转前面n-k个字符。
        		3.  旋转整体

​	

总结：

 1. C++中的substr()函数，第一个参数是开始位置，是size_t，不是iterator，第二个参数是子字符串的长度。 reverse的函数的参数则是iterator

    



