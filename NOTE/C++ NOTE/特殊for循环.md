# 范围 for 循环 (C++11)

## 基本用法

```cpp
vector<int> vec = {1, 2, 3, 4, 5};

for (int num : vec) {     // 每次取一个元素，自动遍历完
    cout << num << " ";
}
// 输出：1 2 3 4 5
```

等价于传统的迭代器写法，但**简洁得多**：
```cpp
// 上面的冒号写法 ≈ 下面这坨
for (auto it = vec.begin(); it != vec.end(); ++it) {
    int num = *it;
    // ...
}
```

## 有 & 和没 & 的区别

| 写法 | 含义 | 效果 |
|:-----|:-----|:-----|
| `for (auto x : v)` | **拷贝**一份 | 改x不影响原值，浪费内存 |
| `for (auto& x : v)` | **引用**（别名） | 改x=改原值，零额外开销 |
| `for (const auto& x : v)` | 只读引用 | 不能改，也不拷贝——**最常用** |

```cpp
string s = "hello";

// 没有&：每个字符都复制一遍（浪费）
for (char c : s) {
    cout << c;            // 只读的话还行
}

// 有&：直接操作原数据（高效）
for (char& c : s) {
    c = toupper(c);       // 直接改原字符串！s变成"HELLO"
}
```

**口诀**：只读用 `const auto&`，要改用 `auto&`，既不读又不改……那你遍历干嘛？
