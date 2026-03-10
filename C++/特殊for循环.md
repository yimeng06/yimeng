C++中的特殊for循环
for (int num : largeData) 是C++11引入的范围for循环（range-based for loop）。它用于遍历容器（如vector、array、list等）或数组中的每一个元素。
相当于：
for (auto it = largeData.begin(); it != largeData.end(); ++it) {
int num = *it;
... // 循环体
}
vector<int> vec = {1, 2, 3, 4, 5};
    for (int num : vec) {
        cout << num << " ";
    }
    // 输出：1 2 3 4 5

string t;(取地址符号的作用)
// 复制版本（没有&）
for (auto row : t) {
    // row = t[0] 时，会：
    // 1. 创建一个新的string对象row
    // 2. 把t[0]的内容复制到row（深拷贝）
    // 3. row有自己的内存空间
    ans += row;
}
// 内存使用：原字符串 + 副本 = 双倍内存！

// 引用版本（有&）
for (auto& row : t) {
    // row = t[0] 时：
    // 1. row只是t[0]的引用（别名）
    // 2. 没有创建新string对象
    // 3. 没有内存复制
    ans += row;
}
// 内存使用：只有原字符串