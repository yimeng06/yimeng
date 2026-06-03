# 构造函数 —— 对象的"出生证明"

**一句话**：创建对象时自动调用的函数，负责初始化。名字必须和类一样，没有返回值。

## 基本规则

| 规则 | 说明 |
|:-----|:-----|
| 名字 = 类名 | 一模一样 |
| 无返回值 | 连 `void` 都不写 |
| 创建时自动调 | 不用手动调用 |
| 可以重载 | 参数不同就行 |
| 不能是虚/静态/const/friend | 这些都不行（但可以是模板）|

---

## 一、构造函数重载 —— 同名不同参数

```cpp
class Problem {
public:
    Problem(int a)    { cout << "int版本" << endl; }
    Problem(double a) { cout << "double版本" << endl; }
};

Problem p1(5);      // int版本
Problem p2(5.0);    // double版本——编译器自动匹配最合适的
```

**带默认值**：参数给个默认值，创建时可以不传。

```cpp
class Person {
public:
    string name;
    int age;

    Person(string n = "Unknown", int a = 0) {   // 默认值！
        name = n;
        age = a;
    }
};

Person p1;                // → Unknown, 0（用默认值）
Person p2("Alice", 25);   // → Alice, 25
```

---

## 二、拷贝构造函数 —— 克隆

用一个已有的对象来创建新对象时触发：

```cpp
Person p1("Alice", 25);
Person p2 = p1;     // 调用拷贝构造（等价于 Person p2(p1)）
// p2 是 p1 的复制品
```

### 浅拷贝 vs 深拷贝

这是**面试必考**的坑！

```cpp
class Array {
    int* a;           // 指针成员！
    int n;
public:
    Array(int size) : n(size), a(new int[n]) {}

    // ❌ 浅拷贝（编译器默认生成的版本）
    // 只是复制指针地址 → 两个对象指向同一块内存！
    // 后果：改一个另一个也变，double free崩溃

    // ✅ 深拷贝（自己写的正确版本）
    Array(const Array& other) {
        n = other.n;
        a = new int[n];              // 自己申请新内存！
        for (int i = 0; i < n; i++)
            a[i] = other.a[i];      // 把值一个个搬过来
    }

    // 赋值运算符也要配套写深拷贝
    Array& operator=(const Array& other) {
        if (this == &other) return *this;  // 防止自己赋值给自己
        delete[] a;                         // 先释放旧的
        n = other.n;
        a = new int[n];
        for (int i = 0; i < n; i++)
            a[i] = other.a[i];
        return *this;                       // 支持链式赋值 a=b=c
    }
};
```

| | 浅拷贝 | 深拷贝 |
|:-:|:-------|:-------|
| **内置类型**（int、string等） | 够用，直接复制值 | 没必要 |
| **指针类型** | ❌ 只复制地址，两个指针指向同一块内存 | ✅ 新建内存，复制内容 |
| **后果** | double free、修改互相影响 | 各自独立，安全 |

> **口诀**：类里有指针？手写深拷贝，否则迟早出事。

---

## 三、什么时候调用哪个？

```cpp
Person p1("Alice", 25);       // 普通构造函数
Person p2(p1);                 // 拷贝构造函数
Person p3 = p1;                // 也是拷贝构造函数！（不是赋值！）
Person p4;                      // 默认构造函数
p4 = p1;                       // 这是赋值运算符 operator=，不是拷贝构造！
```

> **`=` 在声明时是拷贝构造，在声明后是赋值运算符**——这个区别面试常考。

---

## 总结

| 知识点 | 要点 |
|:-------|:-----|
| **基本构造** | 名字=类名，无返回值，创建时自动调用 |
| **重载** | 参数不同即可，支持默认值 |
| **拷贝构造** | 用已有对象创建新对象 |
| **浅拷贝 vs 深拷贝** | 有指针必须深拷贝（new新空间+逐元素复制）|
| **operator=** | 赋值运算符也要配套写深拷贝，记得防自赋值 |
