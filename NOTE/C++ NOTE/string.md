# C++ string —— 比 `char[]` 好用一万倍的字符串

**一句话**：动态管理内存、自带各种方法，不用手动管 `\0`，不用操心缓冲区溢出。

> 头文件：`#include <string>`

---

## 一、创建与基本操作

```cpp
string s1 = "Hello";          // 最常用
string s2("World");           // 构造函数
string s3(5, 'A');            // "AAAAA"：5个'A'
```

| 操作 | 写法 | 说明 |
|:-----|:-----|:-----|
| **拷贝** | `string s3 = s1;` | 深拷贝，各自独立 |
| **赋值** | `s1 = "New";` | 自动调整大小 |
| **拼接** | `s1 + s2` 或 `s1 += s2` | 自动处理内存 |
| **比较** | `s1 == s2`, `s1 < s2` | 字典序比较，直接用运算符 |
| **长度** | `s.length()` 或 `s.size()` | 不包含末尾`\0` |

---

## 二、常用方法速查

```cpp
string s = "Hello World";

s.substr(6, 5);                  // "World"       → 截取子串
s.find("World");                 // 6             → 查找位置（找不到返回 npos）
s.replace(6, 5, "C++");          // "Hello C++"   → 替换
s.insert(5, " Beautiful");      // 中间插入
s.erase(5, 11);                 // 删除一段

// 迭代器（= 智能导游）
s.begin();   // 首字符位置
s.end();     // 末尾的下一个（标志位，不访问）
s.rbegin();  // 反向首字符（最后一个字符）
s.rend();    // 反向结束标志
```

### 迭代器遍历

```cpp
string s = "Hello";

// 正向
for (auto it = s.begin(); it != s.end(); ++it)
    cout << *it;                // H e l l o

// 反向
for (auto it = s.rbegin(); it != s.rend(); ++it)
    cout << *it;                // o l l e H

// 从第2个字符开始
for (auto it = s.begin() + 1; it != s.end(); ++it)
    cout << *it;                // e l l o

// 范围for（最简洁！）
for (char c : s) cout << c;     // H e l l o
```

> `auto` 让编译器自动推断类型——偷懒神器。

---

## 三、容量管理（容易混淆）

```cpp
s.length();         // 实际用了多少（长度）
s.capacity();      // 底层分配了多少空间（容量）
s.reserve(100);    // "给我预留100个房间的酒店"
s.shrink_to_fit(); // "把空房间退掉，省钱"
```

```
长度 =住了几个客人
容量 =酒店总共有多少房间
容量 ≥ 长度 = 还有空房间可用
```

> `push_back` 时如果**长度==容量**，底层会翻倍扩容（代价不小），所以知道大概大小时先 `reserve`。

---

## 四、string 与 char[] 互转

```cpp
// string → char*（只读）
string s = "Hello";
const char* cstr = s.c_str();     // 返回 const char*
const char* cstr2 = s.data();     // 同上

// ⚠️ 修改s后cstr可能失效！（s可能重新分配内存）
s += " World";
// cout << cstr;  ← 可能崩溃！

// 安全做法：自己拷贝一份
char buf[20];
strcpy(buf, s.c_str());

// char[] → string（自动转换）
char arr[] = "World";
string s1 = arr;          // "World"
string s2(arr, 3);        // "Wor"（指定取前3个）
```

---

## 五、输入输出

```cpp
string s;
cin >> s;              // 遇到空格/换行就停（读一个单词）
getline(cin, s);        // 读一整行（包括空格，直到换行符）
cout << s;              // 输出
```

> `cin >> s` 和 `getline` 混用时要注意：`cin` 会留下一个换行符在缓冲区，导致后面的 `getline` 直接读到空行。解决方法：`cin.ignore()` 吃掉那个换行符。

---

## 六、查找所有出现的位置

```cpp
string s = "Hello World! World is amazing. World!";
string sub = "World";
size_t pos = s.find(sub);        // 从头开始找
int count = 0;

while (pos != string::npos) {     // npos = "没找到"
    count++;
    pos = s.find(sub, pos + sub.length());  // 从下一个位置继续找
}
// count = 3
```

> `size_t` 是无符号整数类型（专门表示大小/索引）。`string::npos` 是"未找到"的标记值（实际上是 `-1` 转成无符号后的最大值）。

---

## 总结速查表

| 类别 | 方法 | 作用 |
|:-----|:-----|:-----|
| **增** | `+=`, `append()`, `insert()` | 拼接/插入 |
| **删** | `erase()`, `clear()` | 删除/清空 |
| **查** | `find()`, `substr()` | 查找/截取 |
| **改** | `replace()` | 替换 |
| **量** | `size()`, `capacity()`, `length()` | 大小信息 |
| **转** | `c_str()`, `data()` | 转 char* |

> **口诀**：能用 string 就别用 char[]——自动管内存、不会溢出、方法齐全。除非你在写C接口或者追求极致性能。
