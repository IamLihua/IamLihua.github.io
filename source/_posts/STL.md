---
title: STL笔记
date: 2024-04-09 19:55:40
tags: c++
categories: 
  - note
excerpt: map vector list string set stack
---

# STL笔记

## map

```c++
#include<iostream>
#include<algorithm>
#include<map>

using namespace std;

int main(){
    
    map<int,int> map_test;
    map_test[1]=100;
    map_test[2]=200;
    // 返回键的迭代器
    map<int,int>::iterator i=map_test.find(1);
    cout<<i->second<<endl;//100
    
    // 如果没有就是返回尾指针
    i=map_test.find(-1);
    cout<<(i == map_test.end())<<endl; // 1

    // 遍历
    for(map<int,int>::iterator i=map_test.begin();i!=map_test.end();i++){
        cout<<i->first<<' '<<i->second<<endl;
    }

    // 删除一个元素
    map_test.erase(map_test.find(2));
    // 字典大小
    cout<<map_test.size()<<endl; // 1

    // 清除字典
    map_test.clear();
    cout<<map_test.empty()<<endl; // 1
       
    
    return 0;
}
```

## vector

```c++
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

int main(){
    
    vector<int> vector_test {1,2,3};

    // 在尾部加入
    vector_test.push_back(4);
    for(auto i:vector_test){
        cout<<i<<' '; // 1 2 3 4
    }cout<<endl;
    
    // 在尾部弹出
    vector_test.pop_back();
    for(auto i=vector_test.begin();i!=vector_test.end();i++){
        cout<<*i<<' ';//1 2 3
    }cout<<endl;

    // 删除指定
    vector_test.erase(vector_test.begin());
    cout<<vector_test.size()<<endl;//2

    // 清除
    vector_test.clear();
    cout<<vector_test.empty()<<endl;//1
    return 0;
}
```

排序方式

```c++
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;
bool cmp_greater(int x,int y){
	return x > y;
}

int main(){
    
    vector<int> vector_test {5,4,1,2,3};
    vector<int> v1(vector_test);//注意:对于vector这两个都是深拷贝
    vector<int> v2=vector_test;//注意:对于vector这两个都是深拷贝

    sort(vector_test.begin(),vector_test.end(),greater<int>()); // 从大到小
    sort(v1.begin(),v1.end()); // 默认从小到大
    sort(v2.begin(),v2.end(),cmp_greater);// 从大到小

    vector<int> v3=vector_test;//深拷贝
    // 逆序
    reverse(v3.begin(),v3.end());//从小到大

    for(int i=0;i<v1.size();i++){
        cout<<vector_test[i]<<' '<<v1[i]<<' '<<v2[i]<<' '<<v3[i]<<endl;
    }
    /*
    5 1 5 1
    4 2 4 2
    3 3 3 3
    2 4 2 4
    1 5 1 5
    */
    return 0;
}
```

## list

和vector差不多

```c++
#include<iostream>
#include<algorithm>
#include<list>


using namespace std;

int main(){
    
    list<int> list_test {1,2,3};

    list_test.push_back(4);
    for(auto i:list_test){
        cout<<i<<' '; // 1 2 3 4
    }cout<<endl;
    
    list_test.pop_back();
    for(auto i=list_test.begin();i!=list_test.end();i++){
        cout<<*i<<' ';//1 2 3
    }cout<<endl;

    list_test.erase(list_test.begin());
    cout<<list_test.size()<<endl;//2

    list_test.clear();
    cout<<list_test.empty()<<endl;//1
    return 0;
}
```

和vector的区别：[C++ vector和list的区别 - 迪米特 - 博客园 (cnblogs.com)](https://www.cnblogs.com/shijingjing07/p/5587719.html)

> list\<int>::iterator不支持“+”、“+=”、“<”等
>
> vector\<int>::iterator和list\<int>::iterator都重载了“++”运算符。
>
> 总之，如果需要高效的随机存取，而不在乎插入和删除的效率，使用vector;
>
> 如果需要大量的插入和删除，而不关心随机存取，则应使用list。

## string

```c++
#include<iostream>
#include<algorithm>
#include<string>

using namespace std;


int main(){
    
    string s1("hello");
    string s2("world");

    cout<<(s1>s2)<<endl;//0 按字典序

    s1.push_back(','); //只能插字符
    s1.append("world"); // 或者直接+=
    s2+="!";
    cout<<s1<<endl;//hello,world
    cout<<s2<<endl;//world!

    // 可以用vector的方法遍历
    // ...

    // 字符串查找 替换
    
    string s3("hello world");
    int pos=s3.find("world");
    if(pos!=string::npos){//string::npos是一个特殊的值，表示没有找到
        s3.replace(pos,5,"C++");// 替换5个字符
    }
    cout<<s3<<endl;//hello C++

    //提取子串
    string s6=s3.substr(6,5);// 6到10
    cout<<s6<<endl;//C++
    
    reverse(s1.begin(),s1.end());
    cout<<s1<<endl;//dlrow,olleh
    return 0;
}
```

C++ string 成员函数 `length()` 等同于 `size()`

## set

在C++中，`std::set` 是一个关联容器，它包含唯一元素。`std::set` 底层使用红黑树实现，其中的元素默认按键值**自动升序排序**，并且每个元素的值都是唯一的。下面列出了 `std::set` 的一些常见操作：

1. **插入元素**：

* `insert()`: 向集合中插入一个或多个元素。如果元素已存在，则插入操作不会进行。

```cpp
std::set<int> s;
s.insert(10);
s.insert(20);
s.insert(30);
```
或者使用初始化列表进行批量插入：


```cpp
std::set<int> s = {10, 20, 30};
```
2. **删除元素**：

* `erase()`: 从集合中删除一个元素或一个元素范围。

```cpp
std::set<int> s = {10, 20, 30};
s.erase(20); // 删除元素20
```
3. **查找元素**：

* `find()`: 在集合中查找一个元素，并返回一个迭代器指向它。如果元素不存在，则返回 `end()` 迭代器。

```cpp
std::set<int> s = {10, 20, 30};
auto it = s.find(20);
if (it != s.end()) {
    // 元素存在
}
```
4. **判断集合是否为空**：

* `empty()`: 检查集合是否为空。

```cpp
std::set<int> s;
if (s.empty()) {
    // 集合为空
}
```
5. **获取集合大小**：

* `size()`: 返回集合中元素的数量。

```cpp
std::set<int> s = {10, 20, 30};
std::cout << "Set size: " << s.size() << std::endl; // 输出：Set size: 3
```
6. **遍历集合**：
使用迭代器或基于范围的for循环来遍历集合。


```cpp
std::set<int> s = {10, 20, 30};
for (auto it = s.begin(); it != s.end(); ++it) {
    std::cout << *it << " ";
}
std::cout << std::endl; // 输出：10 20 30
```
或者使用基于范围的for循环：


```cpp
for (const auto& elem : s) {
    std::cout << elem << " ";
}
std::cout << std::endl; // 输出：10 20 30
```
7. **获取集合中的最大和最小元素**：
由于 `std::set` 是有序的，因此可以通过 `begin()` 和 `rbegin()` 直接获取最小和最大元素。


```cpp
std::set<int> s = {10, 20, 30};
int min_elem = *s.begin(); // 获取最小元素
int max_elem = *s.rbegin(); // 获取最大元素
```
8. **其他操作**：
	* `clear()`: 清空集合中的所有元素。
	* `count()`: 返回集合中某个元素的数量（对于 `std::set`，结果要么是0要么是1，因为元素是唯一的）。

这些是 `std::set` 的一些基本操作。当然，`std::set` 还提供了其他成员函数和操作符，但这些是最常用和基本的操作。

示例代码：

```c++
#include<string>
#include<iostream>
#include<set>
using namespace std;

int main(){
    set<int> s;
    s.insert(1);
    s.insert(1);
    s.insert(3);
    cout<<s.size()<<endl;//2

    set<int>::iterator it =s.find(3);
    cout<<*it<<endl;//3

    for(auto i=s.begin();i!=s.end();i++){
        cout<<*i<<' ';//1 3
    }cout<<endl;

    s.clear();
    cout<<s.empty();//1

    return 0;
}
```

感觉和list差不多，就是不能重复，如果希望元素能重复的话，可以用`multiset`

## stack

C++的`stack`是一个容器适配器，它给予程序员栈的功能，即后进先出（LIFO）的数据结构。`stack`不是一个完整的容器，它只是一个封装了另一个容器的对象，通常这个被封装的容器是`deque`，但也可以是其他任何提供了必要操作的容器。

以下是一些C++ `stack`的常用操作：

1. **push()**: 将一个元素压入栈顶。

```cpp
std::stack<int> myStack;  
myStack.push(5);  // 将5压入栈顶
```

2. **pop()**: 移除栈顶的元素。

```cpp
std::stack<int> myStack;  
myStack.push(5);  
myStack.pop();  // 移除栈顶的元素，即5
```

3. **top()**: 返回栈顶的元素，但不移除它。

```cpp
std::stack<int> myStack;  
myStack.push(5);  
int topElement = myStack.top();  // topElement现在是5，但5仍然在栈中
```

4. **size()**: 返回栈中的元素数量。

```cpp
std::stack<int> myStack;  
myStack.push(5);  
myStack.push(10);  
size_t stackSize = myStack.size();  // stackSize现在是2
```

5. **empty()**: 检查栈是否为空，如果为空则返回`true`，否则返回`false`。

```cpp
std::stack<int> myStack;  
bool isEmpty = myStack.empty();  // isEmpty现在是true，因为栈是空的  
myStack.push(5);  
isEmpty = myStack.empty();  // isEmpty现在是false，因为栈中有一个元素
```

## queue

C++的`queue`也是一个容器适配器，它提供了队列的功能，即先进先出（FIFO）的数据结构。与`stack`类似，`queue`也不是一个完整的容器，而是封装了另一个容器（通常是`deque`）的对象。

在队列（Queue）数据结构中，"队头"（Front）和"队尾"（Rear）是两个重要的概念。

1. **队头（Front）**：
   队头是指队列中第一个元素的位置。在先进先出（FIFO）的队列中，队头元素是最早进入队列的元素，也将是第一个被移除的元素。当你调用队列的出队操作（如`pop`或`dequeue`）时，位于队头的元素会被移除。
2. **队尾（Rear）**：
   队尾是指队列中最后一个元素的位置。在队列中，新元素总是被添加到队尾。当你调用队列的入队操作（如`push`或`enqueue`）时，新元素会被添加到队尾。

以下是一些C++ `queue`的常用操作：

1. **push()**: 向队列尾部添加一个元素。

```cpp
std::queue<int> myQueue;  
myQueue.push(5);  // 将5添加到队列尾部
```

2. **pop()**: 移除队列的第一个元素。

```cpp
std::queue<int> myQueue;  
myQueue.push(5);  
myQueue.pop();  // 移除队列的第一个元素，即5
```

3. **front()**: 返回队列的第一个元素，但不移除它。

```cpp
std::queue<int> myQueue;  
myQueue.push(5);  
int frontElement = myQueue.front();  // frontElement现在是5，但5仍然在队列中
```

4. **back()**: 返回队列的最后一个元素，但不移除它（C++11及以后版本）。

```cpp
std::queue<int> myQueue;  
myQueue.push(5);  
int backElement = myQueue.back();  // backElement现在是5，且5仍然在队列中
```

5. **size()**: 返回队列中的元素数量。

```cpp
std::queue<int> myQueue;  
myQueue.push(5);  
myQueue.push(10);  
size_t queueSize = myQueue.size();  // queueSize现在是2
```

6. **empty()**: 检查队列是否为空，如果为空则返回`true`，否则返回`false`。

```cpp
std::queue<int> myQueue;  
bool isEmpty = myQueue.empty();  // isEmpty现在是true，因为队列是空的  
myQueue.push(5);  
isEmpty = myQueue.empty();  // isEmpty现在是false，因为队列中有一个元素
```

与`stack`类似，虽然队列容器适配器提供了对其底层容器的一些访问，但通常最好坚持使用队列自己的成员函数来保持其FIFO特性。在实际编程中，最常用的操作通常是`push()`, `pop()`, `front()`, 和`back()`（如果使用C++11或更新版本）。这些操作直接对应于队列的基本概念和行为。

## deque

`deque`（双端队列）提供了一系列常用操作，这些操作允许你在队列的两端添加或删除元素，以及进行其他相关操作。以下是一些`deque`的常用操作：

1. **push_front(element)**: 在`deque`的前端插入一个元素。

2. **push_back(element)**: 在`deque`的后端插入一个元素。

3. **pop_front()**: 移除`deque`前端的元素。

4. **pop_back()**: 移除`deque`后端的元素。

5. **front()**: 返回对`deque`第一个元素的引用，但不移除该元素。

6. **back()**: 返回对`deque`最后一个元素的引用，但不移除该元素。

7. **size()**: 返回`deque`中元素的数量。

8. **empty()**: 检查`deque`是否为空，如果为空则返回`true`，否则返回`false`。

9. **clear()**: 移除`deque`中的所有元素，使其变为空队列。

10. **begin() / end()**: 返回指向`deque`第一个元素和尾后元素的迭代器。

11. **rbegin() / rend()**: 返回指向`deque`最后一个元素和首前元素的逆向迭代器。

12. **insert(position, element)**: 在指定位置插入一个或多个元素。

13. **erase(position)**: 移除指定位置的元素。

14. **at(index)**: 返回指定索引位置的元素引用。

15. **[]**: 通过索引访问元素，类似于数组的下标操作。

这些操作提供了对`deque`的基本控制和访问功能。需要注意的是，具体的方法和函数名称可能会根据你所使用的编程语言和库有所不同。例如，在C++的STL（Standard Template Library）中，`deque`是一个模板类，提供了上述的方法。在其他语言或库中，可能会有类似的功能，但名称和实现可能略有不同。

下面是一个简单的C++示例，展示了如何使用`deque`的一些基本操作：

```cpp
#include <iostream>
#include <deque>

int main() {
    std::deque<int> myDeque;

    // 添加元素到队尾
    myDeque.push_back(10);
    myDeque.push_back(20);
    myDeque.push_back(30);

    // 添加元素到队首
    myDeque.push_front(5);
    myDeque.push_front(0);

    // 输出deque的内容
    for (int num : myDeque) {
        std::cout << num << " ";
    }
    std::cout << std::endl; // 输出: 0 5 10 20 30

    // 移除队首和队尾的元素
    myDeque.pop_front();
    myDeque.pop_back();

    // 再次输出deque的内容
    for (int num : myDeque) {
        std::cout << num << " ";
    }
    std::cout << std::endl; // 输出: 5 10 20

    return 0;
}
```

## priority_queue

在C++中，可以使用 `std::priority_queue` 来实现优先队列。优先队列是一个容器，其中的元素按照一定的优先级顺序进行排列，具有较高优先级的元素会在队列的前面。

以下是一个简单的示例，展示了如何使用 `std::priority_queue`：

```cpp
#include <iostream>
#include <queue>

int main() {
    // 创建一个整数优先队列，默认是最大堆
    std::priority_queue<int> maxHeap;

    // 插入一些元素
    maxHeap.push(30);
    maxHeap.push(10);
    maxHeap.push(50);
    maxHeap.push(20);

    // 访问队列顶部元素
    std::cout << "Top element of max heap: " << maxHeap.top() << std::endl;

    // 弹出队列顶部元素
    maxHeap.pop();

    // 输出剩余元素
    std::cout << "Elements in max heap after popping: ";
    while (!maxHeap.empty()) {
        std::cout << maxHeap.top() << " ";
        maxHeap.pop();
    }
    std::cout << std::endl;

    return 0;
}
```

这段代码首先创建了一个整数优先队列 `maxHeap`，并将一些整数元素插入其中。然后，它访问并输出了队列中的顶部元素，接着弹出了顶部元素，并输出了弹出后队列中剩余的元素。 

需要注意的是，默认情况下，`std::priority_queue` 是使用 `std::less` 来实现的，因此默认是最大堆。如果要使用最小堆，需要指定比较函数，可以通过传递第三个参数来实现，例如 `std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;` 就是创建一个最小堆。



要自定义比较函数，你需要提供一个函数或者函数对象，该函数或对象用于定义元素之间的比较规则。在C++中，可以使用函数或者函数对象作为比较函数。以下是两种方式的示例：

**方式一：使用函数**(建议不要用 有点看不懂decltype这个推断关键字)

```cpp
#include <iostream>
#include <queue>

// 自定义比较函数，用于最小堆
bool customCompare(int a, int b) {
    return a > b; // 降序排列
}

int main() {
    // 创建一个整数优先队列，使用自定义比较函数
    std::priority_queue<int, std::vector<int>, decltype(&customCompare)> minHeap(customCompare);

    // 插入一些元素
    minHeap.push(30);
    minHeap.push(10);
    minHeap.push(50);
    minHeap.push(20);

    // 输出最小堆中的元素
    std::cout << "Elements in min heap: ";
    while (!minHeap.empty()) {
        std::cout << minHeap.top() << " ";
        minHeap.pop();
    }
    std::cout << std::endl;

    return 0;
}
```

**方式二：使用函数对象（仿函数）**

```cpp
#include <iostream>
#include <queue>

// 自定义比较仿函数，用于最小堆
struct CustomCompare {
    bool operator()(int a, int b) const {
        return a > b; // 降序排列
    }
};

int main() {
    // 创建一个整数优先队列，使用自定义比较仿函数
    std::priority_queue<int, std::vector<int>, CustomCompare> minHeap;

    // 插入一些元素
    minHeap.push(30);
    minHeap.push(10);
    minHeap.push(50);
    minHeap.push(20);

    // 输出最小堆中的元素
    std::cout << "Elements in min heap: ";
    while (!minHeap.empty()) {
        std::cout << minHeap.top() << " ";
        minHeap.pop();
    }
    std::cout << std::endl;

    return 0;
}
```

**优先的大小和比较是反着的，如最大优先用的是less函数比较，最小优先使用的是greater函数进行比较**

后来发现还能这样：

```c++
typedef pair<int, int> PII;//堆里存储距离和节点编号
priority_queue<PII, vector<PII>, greater<PII>> heap;//小根堆
```

在C++中，`std::greater<pair<int, int>>` 是一个功能对象（也称为仿函数），它定义了一个“大于”关系，用于比较两个 `std::pair<int, int>` 对象。

具体来说，`std::greater<T>` 是一个模板类，它提供了一个函数调用操作符 `operator()`，该操作符接受两个类型为 `T` 的参数，并返回一个布尔值，表示第一个参数是否大于第二个参数。

当 `T` 是 `std::pair<int, int>` 时，`std::greater<pair<int, int>>` 就可以用来比较两个 `std::pair<int, int>` 对象。比较的方式是首先比较 `pair` 的第一个元素，如果第一个元素相等，则比较第二个元素。

举个例子，假设你有两个 `pair<int, int>` 对象：`p1 = {3, 2}` 和 `p2 = {2, 5}`。使用 `std::greater<pair<int, int>>` 来比较它们时，会先比较第一个元素。因为 `p1` 的第一个元素（3）大于 `p2` 的第一个元素（2），所以 `std::greater<pair<int, int>>()(p1, p2)` 会返回 `true`。

## 关于深拷贝

在构造函数和赋值的时候都是深拷贝

```c++
#include<iostream>
#include<algorithm>
#include<string>
#include<list>

using namespace std;

int main(){
    
    string s1("hello");
    string s2(s1);
    reverse(s2.begin(),s2.end());
    cout<<s1<<"---"<<s2;//hello---olleh
    cout<<endl;

    list<int> l1{1,2,3};
    list<int> l2=l1;
    reverse(l2.begin(),l2.end());
    for(int i:l1){
        cout<<i<<' ';// 1 2 3
    }
    return 0;
}
```

## 关于去重

```c++
#include <iostream>  
#include <algorithm>  
#include <vector>  
  
int main() {  
    std::vector<int> myVector = {3, 4, 5, 6, 3, 4, 5, 33, 55};  
      
    // 排序  
    std::sort(myVector.begin(), myVector.end());  
      
    // 去重  
    std::vector<int>::iterator dupilcated = std::unique(myVector.begin(), myVector.end());  

    for(int i:myVector){
        std::cout<<i<<'*';//3*4*5*6*33*55*6*33*55
    }
      
    // 擦除重复元素  
    myVector.erase(dupilcated, myVector.end());  
      
    // 输出结果  
    for (const auto& num : myVector) {  
        std::cout << num << " ";  //3 4 5 6 33 55
    }  
      
    return 0;  
}
```

> 为什么这种方法可以把所有重复的元素都去除了，而不是只去除一个？

> 这种方法能够去除所有重复的元素，而不仅仅是去除一个，是因为它结合了排序和去重的操作。让我们详细分析一下这个过程：
>
> 1. **排序**：首先，`std::sort`函数对`vector`中的所有元素进行排序。排序是一个关键的步骤，因为它确保了所有重复的元素都相邻排列在一起。如果没有排序，那么重复的元素可能会分布在`vector`的不同位置，这使得去重变得更加复杂。
> 2. **去重**：接下来，`std::unique`函数被用来去重。`std::unique`函数的工作原理是，它遍历排序后的`vector`，并将每个不重复的元素移动到它应该在的位置（即它首次出现的位置）。同时，它将所有重复的元素移动到`vector`的末尾，但并不从容器中删除它们。`std::unique`返回一个迭代器，指向最后一个不重复元素之后的位置。
>
> 由于`vector`已经被排序，`std::unique`能够确保每个不重复的元素只出现一次，并且所有重复的元素都被移动到`vector`的末尾。
>
> 1. **擦除**：最后，使用`vector`的`erase`成员函数来删除从`std::unique`返回的迭代器位置到`vector`末尾的所有元素。这些元素就是所有重复的元素，因为`std::unique`已经将它们移动到了`vector`的末尾。
>
> 通过这种方式，所有重复的元素都被有效地从`vector`中删除了，只剩下不重复的元素。
>
> 需要注意的是，`std::unique`函数仅适用于已排序的范围。如果`vector`没有事先排序，那么`std::unique`将不能正确地去除所有重复的元素。因此，在使用`std::unique`之前进行排序是至关重要的一步。
