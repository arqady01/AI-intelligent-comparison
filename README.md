# AI-intelligent-comparison
AI工具智商对比

```cpp
class singoton {
public:
    //ctor为私有导致无法创建对象，static可以保证不创建对象就能调用函数
    static singoton* getPtr() {
        if (nullptr == sn) sn = new singoton();
        return sn;
    }
    void print() { std::cout << "singoton mode\n"; }
private:
    struct CS {
    public:
        ~CS() {
            delete singoton::sn;
            singoton::sn = nullptr;
        }
    };
    static CS cs;
private:
    singoton() {}; //ctor设为私有
    //未创建对象就调用getPtr()是ok的，但是*sn也要提前创建，所以设为静态
    static singoton* sn;
};
singoton* singoton::sn = nullptr; //懒汉式
singoton::CS singoton::cs; //必须初始化，不然程序结束无法析构CS
int main() {
    //创建类的唯一对象，相当于 singoton* ptr = new singoton();
    singoton* ptr = singoton::getPtr();
    //因为sn不为空，调用getPtr，*sn也会原封不动的传出来
    singoton* ptr2 = singoton::getPtr();
    ptr->print();
}
```
