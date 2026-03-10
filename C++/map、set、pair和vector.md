1、固定小本本
const pair<int, string> valueSymbols[]
这是一个固定数组，元素是 const pair<int, string> 类型。

特点：
大小固定，声明时必须确定或由初始化推导
元素不可修改（const）
常用于存储编译时已知的映射关系
内存分配在栈上（如果全局则可能在数据段）
例：// 罗马数字映射
const pair<int, string> valueSymbols[] = {
    {1000, "M"}, {900, "CM"}, {500, "D"}, 
    {400, "CD"}, {100, "C"}, {90, "XC"}
};

// 遍历
for (const auto& p : valueSymbols) {
    cout << p.first << " -> " << p.second << endl;
}

2、智能词典
unordered_map<char, int> m
这是一个哈希表，提供键值对映射。

特点：
查找速度快（平均O(1)）
元素无序（插入顺序不保证）
键唯一
动态大小，可随时插入删除
内存分配在堆上
例：// 字符频率统计
unordered_map<char, int> m;
string s = "hello";
for (char c : s) {
    m[c]++;  // 自动插入或更新
}

// 查找
if (m.find('h') != m.end()) {
    cout << "h出现了" << m['h'] << "次" << endl;
}

而unordered_set<int> s
这是一个哈希集合，只存储唯一键。
与map的区别如下
unordered_set<string> vipMembers = {"Alice", "Bob", "Charlie"};
map：
1、键值对映射
2、可动态调整大小
3、保持插入顺序
4、内存分配在堆上
5、可修改元素（set不可以直接修改但可以插入insert和删除erase）
unordered_map<string, string> dictionary = {
    {"apple", "苹果"},
    {"banana", "香蕉"}
};

3、智能容器
智能指针：
1、自动管理内存
vector
这是一个动态数组。

特点：

连续内存存储
支持随机访问（O(1)）
可动态调整大小
保持插入顺序

示例：
vector<int> nums = {1, 2, 3, 4, 5};
// 添加元素
nums.push_back(6);
// 随机访问
cout << nums[2] << endl;  // 3
// 遍历
for (int num : nums) {
    cout << num << " ";
}