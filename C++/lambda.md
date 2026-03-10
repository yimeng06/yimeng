lambda表达式是一个“就地定义、用完即走”的临时函数

主要有三段式[捕获列表] (参数列表) { 函数体 }

1、[捕获列表]：“它能用到外界的什么？”
[]：什么都不用。
[&]：“我全都要！” 以引用方式使用所有外部变量（最省事，但需注意原变量要存活）。
[=]：以拷贝方式使用所有外部变量。
[a, &b]：只要 a（拷贝）和 b（引用）。

2、(参数列表)：“调用它时需要给它什么？” 和普通函数参数一样。

3、{ 函数体 }：“它拿到参数后具体做什么？” 写逻辑的地方。

例如：
auto dfs = [&](this auto&& dfs, TreeNode* node, int depth) -> void {
    // [&] : 以引用方式捕获所有外部变量
    // this auto&& dfs : 显式对象参数，命名为dfs
    // TreeNode* node, int depth : 普通参数
    // -> void : 返回类型
};

也可以用来快速定义条件
1、这是在外面定义一个函数，然后在main函数中调用。
// 跑到外面去定义一个函数
bool cmp_by_last_digit(int a, int b) {
    return (a % 10) < (b % 10);
}

int main() {
    vector<int> nums = {12, 43, 25, 71};
    // 调用时，把函数名字传进去
    sort(nums.begin(), nums.end(), cmp_by_last_digit);
}

2、这是使用lambda表达式来定义一个函数。
int main() {
    vector<int> nums = {12, 43, 25, 71};
    // 就地创建一个“临时工”Lambda来告诉sort怎么比
    sort(nums.begin(), nums.end(),
              [](int a, int b) { // 看，规则就在这里！
                  return (a % 10) < (b % 10);
              });
}

同时引申出一题二叉树的右视图
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        auto dfs = [&](this auto&& dfs,TreeNode* node,int depth)-> void{
            if(node==nullptr)
            {
                return;
            }
            if(depth==ans.size())//深度首次遇到
            {
                ans.push_back(node->val);
            }
            dfs(node->right,depth+1);//先递归右
            dfs(node->left,depth+1);
        };
        dfs(root,0);
        return ans;
    }
};
这里面之所以带三个参数是因为// 定义：Lambda说，我需要三个信息才能工作：
// 1. 我自己的引用（用于递归自调）， 2. 一个节点， 3. 一个深度
auto dfs = [&](this auto&& self, TreeNode* node, int depth) -> void {
    // ...
    self(node->left, depth + 1); // 用“self”调用自己
    // ...
};

并且定义完lambda必须加;，定义函数不需要但lambda是变量定义语句，所以需要加上;

// 1. 定义一个普通函数 —— 这是“函数定义”，不需要分号。
int add(int a, int b) {
    return a + b;
}

// 2. 定义一个Lambda，并赋值给变量 —— 这是“变量定义语句”，需要分号。
auto add_lambda = [](int a, int b) -> int {
    return a + b;
}; // <--- 看，分号在这里！

// 3. 如果你想直接调用一个临时的Lambda，也需要分号结束调用语句。
int result = [](int a, int b) { return a + b; }(5, 3); // <--- 分号在这里！