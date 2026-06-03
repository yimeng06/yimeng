 # C++ 笔记

## 基本输入输出

### 输出
```cpp
// 输出文本
cout << "文本" << endl;  // endl 相当于换行符

// 输出变量
cout << a << endl;

// 输出多个内容，用空格分隔
cout << a << " " << b << endl;
```

### 输入
```cpp
// 输入单个值
cin >> a;

// 输入多个值（会自动跳过空白符）
cin >> a >> b;
// 可以处理如下输入格式：
// 2 4 （一行输入）
// 2
// 4 （多行输入）
```

## 三元运算符

### 语法
```cpp
条件 ? 表达式1 : 表达式2
```

### 功能
- 如果条件为真，则整个表达式的值为表达式1
- 如果条件为假，则整个表达式的值为表达式2

### 示例：比较大小
```cpp
c = (a > b ? a : b);  // 取a和b中的较大值
```

## C++ 模板结构

```cpp
#include <iostream>
using namespace std;  // 使用标准命名空间，避免每次使用std::前缀

int main() {
    // 代码逻辑
    
    system("pause");  // 暂停程序，等待用户输入
    return 0;  // 返回0表示程序正常结束
}
```

## const 与 #define 的区别

### 定义方式
```cpp
// const 定义
const type name = value;

// #define 定义
#define NAME value
```

### 关键区别

#### 1. 类型安全性
- **#define**：只是简单的文本替换，没有类型检查
- **const**：有明确的类型，是类型安全的

#### 2. 运算优先级
```cpp
// #define 示例
#define MAX 10+10

int main() {
    int value = 2 * MAX;  // 实际计算：2*10+10=30
    // 期望结果：2*(10+10)=40
    return 0;
}

// const 示例
const int MAX = 10+10;

int main() {
    int value = 2 * MAX;  // 正确计算：2*(10+10)=40
    return 0;
}
```

#### 3. 作用域
- **#define**：没有作用域概念，从定义点到文件末尾有效（除非被#undef取消）
- **const**：有作用域，可以定义在全局、命名空间、类内部或函数内部

#### 4. 本质
- **#define**：简单的"文字替换"
- **const**：真正的"类型安全变量"