# C++ 类 (class)

---

## 一、类的基本结构

```cpp
class Pointer {
private:
    int x, y;              // 私有财产，外人别想碰

public:
    void setX(int x) {
        this->x = x;       // this->：区分"我的x"和"传进来的x"
    }
};
```

**三个访问权限**（谁能动我的东西）：

| 关键字 | 自己 | 儿子 | 邻居 |
|:------:|:----:|:----:|:----:|
| `public` | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ❌ |
| `private` | ✅ | ❌ | ❌ |

> 不写默认 `private`——最抠门，谁都不给看

---

## 二、访问权限示例

用一家子来比喻最清楚 👇

```cpp
class Family {
protected:
    int safeBox;            // 保险柜：父子都能动
private:
    int personalDiary;      // 私人日记：只有我能看
public:
    int frontDoor;          // 大门敞开：谁爱进谁进
    int sun(int x) {
        personalDiary = x;
        return personalDiary;
    }
};

// 儿子的家（派生类）
class Son : public Family {
    void check() {
        frontDoor = 1;     // ✅ 大门随便进
        safeBox = 2;       // ✅ 保险柜能开
        // personalDiary=3;// ❌ 日记？没门！编译错误
    }
};

// 邻居（外部代码）
int main() {
    Family f;
    f.frontDoor = 10;      // ✅ 大门随便进
    f.sun(8);              // ✅ 走正规途径
    // f.safeBox = 20;     // ❌ 保险柜？你算老几
    // f.personalDiary=30; // ❌ 日记？更别想了
}
```

**一句话**：`public`是大门，`protected`是保险柜，`private`是日记本。

---

## 三、继承与构造函数

儿子出生前，爹得先存在——**构造顺序：先父后子**

```cpp
class Base {           // 爹
private:
    int x, y;
public:
    Base(int x, int y) { this->x = x; this->y = y; }
};

class Sub : public Base {  // 儿子
private:
    int z;
public:
    Sub(int x, int y, int z) : Base(x, y) {  // 先把爹造出来
        this->z = z;                          // 再造自己
    }
};
```

`: Base(x,y)` 就是告诉编译器："**先把我爹初始化了，再来管我**"。不写？编译器会骂你的。

---

## 四、this 指针

当参数名和成员变量撞名了怎么办？

```cpp
void setX(int x) {        // 这个x是外来户
    this->x = x;   // this->x 是我自己的，右边的x是传进来的
}
```

`this` 就是指着自己鼻子说：**"我的是我的，你的是你的"**。

---

## 总结

| 知识点 | 一句话 |
|:-------|:-------|
| **访问控制** | public > protected > private（越来越小气）|
| **继承语法** | `class Son : public Father`（认爹）|
| **构造顺序** | 爹先来，儿后到（初始化列表）|
| **this指针** | "这是我的，那是你的"——解决重名冲突 |
