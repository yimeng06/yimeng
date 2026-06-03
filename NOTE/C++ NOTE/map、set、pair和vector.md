# STL 容器速查：pair / map / set / vector

## 一、pair —— 两个值的"连体婴"

```cpp
pair<int, string> p = {42, "hello"};
cout << p.first;    // 42
cout << p.second;   // "hello"
```

**用途**：map的元素类型、函数返回两个值、临时捆绑两个数据。

> 常配合 `auto [a, b]` 结构化绑定使用，拆开更方便。

---

## 二、vector —— 动态数组（最常用的容器）

```cpp
vector<int> v = {1, 2, 3};
v.push_back(4);          // 尾部插入：{1,2,3,4}
v[0];                    // 随机访问 O(1)
v.size();                // 元素个数
for (int x : v) { }      // 遍历
```

| 特点 | 说明 |
|:-----|:-----|
| 连续内存 | 跟普通数组一样，缓存友好 |
| 动态大小 | 满了自动扩容（翻倍）|
| 随机访问 O(1) | `v[i]` 直接定位 |
| 尾插 O(1) | 头插/中间插入 O(n)，要挪位 |

---

## 三、map vs unordered_map —— 字典

| | `map`（红黑树）| `unordered_map`（哈希表）|
|:-:|:-------------|:----------------------|
| **底层** | 红黑树 | 哈希表 |
| **有序？** | ✅ 按key排序 | ❌ 无序 |
| **查找** | O(log n) | O(1) 平均 |
| **用法** | 需要有序时用 | 只管查，追求速度 |

```cpp
unordered_map<string, int> m;
m["apple"] = 1;           // 插入/修改
m["banana"] = 2;
cout << m["apple"];       // 1
m["apple"]++;             // 修改值
if (m.count("pear")) { }  // 判断存不存在
```

> `m[key]` 不存在时会**自动插入默认值0**！只想查不改？用 `m.find(key) != m.end()` 或 `m.count(key)`。

---

## 四、set vs unordered_set —— 集合

| | `set`（红黑树）| `unordered_set`（哈希表）|
|:-:|:-------------|:----------------------|
| **有序？** | ✅ 自动去重+排序 | ❌ 去重但无序 |
| **查找** | O(log n) | O(1) |
| **能改元素？** | ❌ 删了重新插入 | ❌ 同左 |

```cpp
unordered_set<string> s = {"Alice", "Bob"};
s.insert("Charlie");       // 插入
s.erase("Alice");           // 删除
s.count("Bob");             // 存在返回1，否则0
```

> set/map的区别：**map存键值对，set只存键**。一个像字典，一个像花名册。

---

## 五、如何选择？

```
需要键值映射？
├─ 是 → map / unordered_map
│      ├─ 需要有序 → map
│      └─ 只要快 → unordered_map ⭐ 最常用
└─ 否 → 只存唯一值？
       ├─ 是 → set / unordered_set
       │      ├─ 需要有序 → set
       │      └─ 只要快 → unordered_set
       └─ 否 → 允许重复？
              └─ 是 → vector（最简单粗暴）
```

## 总结对比

| 容器 | 存什么 | 有序？ | 查找 | 底层 |
|:-----:|:-------|:-----:|:----:|:-----|
| **vector** | 一列数 | ✅ 插入序 | O(1)下标 | 动态数组 |
| **map** | 键→值 | ✅ 按key排 | O(log n) | 红黑树 |
| **unordered_map** | 键→值 | ❌ | O(1) | 哈希表 |
| **set** | 键 | ✅ 按key排 | O(log n) | 红黑树 |
| **unordered_set** | 键 | ❌ | O(1) | 哈希表 |

> **面试口诀**：要有序用红黑树(map/set)，要速度用哈希(unordered_)，要简单用vector。
