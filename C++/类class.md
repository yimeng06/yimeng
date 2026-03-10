C++中的类class
class Pointer {

    private:
        int x;  // x 坐标
        int y;  // y 坐标

    public:
        void setX(int x) {
            this->x = x;// this->（因为命名冲突）
        }
}


#include <iostream>
using namespace std;

class Family {
protected:
    int safeBox;        // 保险柜，自己和儿子能用
private:
    int personalDiary;  // 私人日记，只有自己能用(不设置时都默认为private)
public:
    int frontDoor; 
    int sun(int x)
    {
        personalDiary = x;
        cout << personalDiary << endl;
        return personalDiary;
    }// 公共的大门，谁都可以走

};

// 这是儿子的家（派生类），继承自父亲
class Son : public Family {
    void CheckThings() {
        frontDoor = 1;   // ✅ 没问题：公共大门谁都能用
        safeBox = 2;     // ✅ 没问题：保险柜家人能用
        // personalDiary = 3; // ❌ 编译错误！儿子的类不能访问父亲的私人日记
    }
};

// 这是一个邻居（类外部的代码）
int main(){
    Family f;
    f.frontDoor = 10; // ✅ 没问题：公共大门
    f.sun(8);
    cout << f.sun(6) << endl;
    // f.safeBox = 20;   // ❌ 编译错误！邻居不能动人家保险柜
    // f.personalDiary = 30; // ❌ 编译错误！邻居更不能看人家日记
}

关于父子的例子：
class Base {
 
    private:
        int x;
        int y;
 
    public:
        Base(int x, int y) {
            this->x = x;
            this->y = y;
        }
 
        int getX() {
            return x;
        }
 
        int getY() {
            return y;
        }
 
}
 
 class Sub : public Base {
 
    private:
        int z;
 
    public:
        Sub(int x, int y, int z) : Base(x, y) { //先继承父类的构造函数，意思就是先初始化基类Base
          
            this->z = z; //再构造自己的成员变量
        }

 }
