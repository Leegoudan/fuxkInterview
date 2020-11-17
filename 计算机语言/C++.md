# 1. 智能指针和线程安全
- 智能指针提供和内置类型（值语义）相同级别的线程安全（引用计数是**原子操作**）。即一个实例可以被多个线程读，同一对象的多个实例可以被多个线程写。智能指针本身的构造，析构，赋值（拷贝构造），reset()都是线程安全的。
例子：
```
// shared_ptr<int> p(new int(1));
// shared_ptr<int> p1(p);

//thread A
p1.reset();

//thread B
p.reset(new int(2));  //OK

//thread C
p = p1; //error, p1 read/write
```
