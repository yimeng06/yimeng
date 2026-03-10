迭代器
（s.end() 返回指向最后一个元素之后的位置的迭代器，也就是说，它是一个“尾后”迭代器，不指向任何有效的元素。）
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    // 将数组转换为vector
    vector<int> vec(arr, arr + size);
   //使用正向迭代器
 for (auto it = vec.begin(); it != vec.end(); ++it) {
        cout << *it << " ";
    // 使用反向迭代器
    for (auto it = vec.rbegin(); it != vec.rend(); ++it) {
        cout << *it << " ";
    }
    // 输出: 5 4 3 2 1
    
    return 0;
}
}
这使用了vector的范围构造函数，其格式是：
vector<T> vec(iterator_start, iterator_end);
具体含义
cpp
int arr[] = {1, 2, 3, 4, 5};
int size = 5;

vector<int> vec(arr, arr + size);
这表示：
•	arr：指向数组第一个元素的指针（&arr[0]）
•	arr + size：指向数组最后一个元素的下一个位置的指针（&arr[5]）
•	复制的范围是：[arr, arr + size) - 左闭右开区间

这个是vector自己的迭代器，可以用来遍历vector中的元素。
vector<int> vec = {10, 20, 30, 40, 50};

// 创建迭代器
vector<int>::iterator it;

// 迭代器指向第一个元素
it = vec.begin();
cout << *it;  // 输出：10

// 移动到下一个元素
it++;
cout << *it;  // 输出：20

// 移动到最后一个元素
it = vec.end() - 1;
cout << *it;  // 输出：50

/*vector里面还有修改函数*/
修改器: 
clear(): 移除所有元素，使 vector 变为空。
insert(const_iterator pos, const T&amp; value): 在 pos 位置插入一个元素。
emplace(const_iterator pos, Args&amp;&amp;... args): 在 pos 位置构造一个元素。
erase(const_iterator pos): 移除 pos 位置的元素。
push_back(const T&amp; value): 在 vector 的末尾添加一个元素。
emplace_back(Args&amp;&amp;... args): 在 vector 的末尾构造一个元素。
pop_back(): 移除 vector 的最后一个元素。
resize(size_t n, T value = T()): 调整 vector 的大小到 n，如果需要则用 value 填充。
swap(vector&amp; other): 交换两个 vector 的内容。
sort(a.begin(), a.end());  // 对 vector a 中的所有元素进行升序排序