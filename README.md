# AI-intelligent-comparison
AI工具智商对比

```cpp
std::mutex mtx; //不放到类内，是因为静态成员函数只能访问静态成员，除非也定义成静态
class singoton {
public:
    //ctor为私有导致无法创建对象，static可以保证不创建对象就能调用函数
    static singoton* getPtr() {
        std::unique_lock<std::mutex> lck(mtx); //自动上下锁
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
void test() {
    std::cout << "start\n";
    singoton* ptr = singoton::getPtr();
    ptr->print();
    std::cout << "end\n";
}
int main() {
    std::thread(test).join();
    std::thread(test).join();
}
```
