## C++中的string
First使用string需要引用头文件#include<string>
String比char a[]简单
#include <string>
string s1 = "Hello";               // 字符串对象
string s2("World");                // 构造函数初始化
string s3(5, 'A');              // 5个'A'字符："AAAAA"
并且运行内存运行时动态分配，内存大管理和大小调整都是自动的不用担心超出；
1、拷贝
string s1 = "Hi";
string s2("Hello");
string s3 = s1;  // 拷贝
2、赋值
string s1;
s1 = "New Text";         // 自动调整大小
s1 = s2;                 // 深拷贝
3、连接
string s1 = "Hello";
string s2 = " World";
s1 += s2;                // 自动处理内存
s1 = s1 + s2;            // 连接运算
4、比较
if(s1 == s2) {
    // 相等
}
if(s1 < s2) {
    // s1在字典序中小于s2
}
5、长度获取（不包括末尾空字符）
int len = s1.length();   // 直接获取
int len = s1.size();     // 同上


同时除了这些他还有很多自己的独有特性
1、
string s = "Hello World";

s.substr(6, 5);        // "World" - 子串
s.find("World");       // 6 - 查找位置
s.replace(6, 5, "C++"); // "Hello C++" - 替换
s.insert(5, " Beautiful"); // "Hello Beautiful World"
s.erase(5, 11);        // "Hello World" -> "Hello"
s.begin() ;           //指向字符串的第一个字符
s.end();            //指向字符串最后一个字符的下一个位置（结束标志）
s.rbegin();          //指向字符串的最后一个字符（即反向开始）
s.rend();            //指向字符串第一个字符的前一个位置（即反向结束,注意：索引-1，实际上我们不会访问，只是一个标记）

2、迭代器支持（迭代器 = 智能指针，它像是一个"导游"，带你遍历容器（如string）中的所有元素。）
string s = "Hello";
for(auto it = s.begin(); it != s.end(); ++it) {
    cout << *it;  // 输出: H e l l o

}
//还可以反向输出
for(auto it = s.rbegin(); it != s.rend(); ++it) {
    cout << *it << " ";  // 输出: o l l e H
}
//这是int型数组的情况下
Int arr[];
for (auto it = std::rbegin(arr); it != std::rend(arr); ++it) 
    {
        cout << *it << " ";
    }

//还可以指定输出例如从第二个字符开始输出
for(auto it = s.begin()+1; it != s.end(); ++it) {
    cout << *it;  // 输出: e l l o

}


for(char c : s) {      // 范围for循环
    cout << c;
}
//其中auto 是C++11引入的自动类型推断关键字，让编译器自动推断变量的类型。
例如
auto number = 42;           // 推断为int
auto price = 19.99;         // 推断为double  
auto name = "John";         // 推断为const char*
auto result = calculate();  // 自动推断函数返回类型

同时这里面引入了一个新概念就是范围for循环这个也是C++特有的；
公式为for(元素类型 变量名 ：集合){
      //循环体
}
例子：
int arr[] = {1, 2, 3, 4, 5};
for(int num : arr) {
    cout << num << " ";  // 输出: 1 2 3 4 5
}
还可以快捷的修改字符串内容（使用引用）
text=”hello world”
for(char& c : text) {//这里面加了&符号表示会改变集合里面的值
        c = toupper(c);    // 转换为大写
    }
    cout << text << endl;  // 输出: HELLO WORLD

3、容量管理
s.capacity();    // 当前分配的内存容量
s.reserve(100);  // 预分配内存
s.shrink_to_fit(); // 减少内存使用

•	长度 = 用了多少
•	容量 = 总共能装多少
•	容量 > 长度 = 还有空房间

4、char和string的相互转换
（1）string--char[]
string s = "Hello";
const char* cstr = s.c_str();    // 只读访问
const char* cstr2 = s.data();    // 同上

还有要注意修改s后cstr可能失效
s += " World";  // 可能导致内存重新分配
cout << cstr << endl;  // 可能崩溃或输出错误数据

// 如果需要修改，需要拷贝
char buffer[20];
strcpy(buffer, s.c_str());

(2)char[]--string
char arr[] = "World";
string s1 = arr;                 // 自动转换
string s2(arr, 3);               // "Wor" - 指定长度

string的输入
string s;
cin >> s;                       // 遇到空格和换行停止
getline(cin, s);               // 读取整行，无长度限制
输出（有一个跟它相似的cin.getline(str, size)只能给char用这个是读取直到遇到换行符或达到最大长度）

cout << s;                        // string

**注意：string的输入输出都是以空格为分隔符，如果需要换行符，需要自己加上**

5、关于查找字符串位置的方法以及记录字符串出现次数
string s = "Hello World! This is a World of C++ programming. World is amazing.";
    string substring = "World";
    int count = 0;
    size_t pos = s.find(substring); // 初始化查找位置，第一次从0开始 //size_t表示是一个无符号整数类型，通常用于表示对象的大小或数组的索引

while (pos != string::npos) // string::npos是std::string 类中的一个静态常量，表示“未找到”或“无效位置”
{
        count++;
        // 从当前找到的子串的下一个位置开始新的查找
        pos = s.find(substring, pos + substring.length());
}
