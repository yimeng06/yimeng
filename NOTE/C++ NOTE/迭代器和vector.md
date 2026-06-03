# 迭代器和Vector

## 迭代器

**注意**：`vec.end()` 返回指向最后一个元素之后的位置的迭代器，也就是说，它是一个"尾后"迭代器，不指向任何有效的元素。

### 基本使用示例

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    // 将数组转换为vector
    vector<int> vec(arr, arr + size);
    
    // 使用正向迭代器
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        cout << *it << " ";
    }
    
    // 使用反向迭代器
    for (auto it = vec.rbegin(); it != vec.rend(); ++it) {
        cout << *it << " ";
    }
    // 输出: 5 4 3 2 1
    
    return 0;
}
```
}
### Vector范围构造函数

使用了vector的范围构造函数，其格式是：

```cpp
vector<T> vec(iterator_start, iterator_end);
```

**具体含义**：

```cpp
int arr[] = {1, 2, 3, 4, 5};
int size = 5;

vector<int> vec(arr, arr + size);
```

这表示：
- `arr`：指向数组第一个元素的指针（&arr[0]）
- `arr + size`：指向数组最后一个元素的下一个位置的指针（&arr[5]）
- 复制的范围是：[arr, arr + size) - 左闭右开区间

### Vector迭代器操作

Vector自己的迭代器，可以用来遍历vector中的元素。

**迭代器类型**：
- `iterator`：正向迭代器
- `const_iterator`：常量正向迭代器（只读）
- `reverse_iterator`：反向迭代器
- `const_reverse_iterator`：常量反向迭代器（只读）

```cpp
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

// 常量迭代器（只读）
vector<int>::const_iterator cit = vec.cbegin();
// *cit = 100;  // 错误：不能修改
```

## Vector修改器

Vector里面还有修改函数：

- `clear()`：移除所有元素，使 vector 变为空
- `insert(const_iterator pos, const T& value)`：在 pos 位置插入一个元素
- `insert(const_iterator pos, size_t count, const T& value)`：在 pos 位置插入 count 个相同的元素
- `insert(const_iterator pos, iterator first, iterator last)`：在 pos 位置插入范围内的元素
- `emplace(const_iterator pos, Args&&... args)`：在 pos 位置构造一个元素
- `erase(const_iterator pos)`：移除 pos 位置的元素
- `erase(const_iterator first, const_iterator last)`：移除范围内的元素
- `push_back(const T& value)`：在 vector 的末尾添加一个元素
- `emplace_back(Args&&... args)`：在 vector 的末尾构造一个元素
- `pop_back()`：移除 vector 的最后一个元素
- `resize(size_t n, T value = T())`：调整 vector 的大小到 n，如果需要则用 value 填充
- `swap(vector& other)`：交换两个 vector 的内容

## Vector访问函数

- `size()`：返回元素数量
- `empty()`：检查是否为空
- `front()`：返回第一个元素的引用
- `back()`：返回最后一个元素的引用
- `at(size_t pos)`：返回指定位置元素的引用（带边界检查）
- `operator[](size_t pos)`：返回指定位置元素的引用（不带边界检查）

## 排序

```cpp
sort(a.begin(), a.end());  // 对 vector a 中的所有元素进行升序排序
sort(a.rbegin(), a.rend());  // 降序排序
sort(a.begin(), a.end(), greater<int>());  // 使用greater函数对象降序排序
```