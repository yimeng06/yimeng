构造函数
•	构造函数是一种特殊的成员函数，用于在创建对象时初始化对象。
•	构造函数的名称与类名完全相同。
•	构造函数没有返回类型（连void也没有）。
•	构造函数在对象创建时自动调用。
例子：
class Problem {
public:
    Problem(int a) { cout << "int版本" << endl; }
    Problem(double a) { cout << "double版本" << endl; }
};

int main() {
    Problem p1(5);      // 明确调用int版本
    Problem p2(5.0);    // 明确调用double版本
    // Problem p3(5.5f); // 如果有float参数，可能产生歧义
    
    return 0;
}
class Person {
public:
    string name;
    int age;
    
    // 构造函数确保对象正确初始化
    Person(string n = "Unknown", int a = 0) {
        name = n;
        age = a;
    }
    
    void show() {
        cout << name << " " << age << endl;
    }
};

int main() {
    Person p1;              // 使用默认值
    Person p2("Alice", 25); // 使用指定值
    p1.show();              // 输出: Unknown 0
    p2.show();              // 输出: Alice 25
    return 0;
}
还有构造拷贝函数
// 拷贝构造函数
    Person(const Person &other) {
        name = other.name;
        age = other.age;
    }
    当对象为指针时需要深拷贝，否则会出现指针指向同一内存地址，修改其中一个会影响另一个。或者重复释放，和丢失原值
    // 深拷贝构造函数
    Array(Array &array) {
        if (this != &other) {  // 防止自赋值
            delete[] a;
            n = array.getlen();
            a = new int[n];
            for (int i = 0; i < n; i++) {   
            a[i] = array.get(i);
            }
        }
    }
// 重载赋值运算符
    Person& operator=(const Person &other) {
        if (this == &other) return *this; // 防止自我赋值
        name = other.name;
        age = other.age;
        return *this;
    }
int main() {
    Person p1("Alice", 25);
    Person p2 = p1; // 调用拷贝构造函数,也可以这样调用Person p2(p1); 调用拷贝构造函数，使用括号但不像函数调用
    p1.show();        // 输出: Alice 25
    p2.show();        // 输出: Alice 25
    return 0;
}

// 构造函数的一些注意事项
•	构造函数不能返回值，只能初始化对象。
•	构造函数可以被重载。
class MyClass {
public:
    MyClass();              // 默认构造函数
    MyClass(int x);         // 带参构造函数
    MyClass(int x, int y);  // 另一个重载
};
•	构造函数不能被虚函数覆盖。
•	构造函数不能被const成员函数覆盖。
•	构造函数不能被静态成员函数覆盖。
•	构造函数不能被友元函数覆盖。
•	构造函数可以是模板构造函数。
class MyClass {
public:
    template<typename T>
    MyClass(T param) { /* 模板构造函数 */ }
};
