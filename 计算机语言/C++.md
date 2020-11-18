# 1. 智能指针和线程安全
智能指针提供和内置类型（值语义）相同级别的线程安全。即一个shared_ptr可以被多个线程读，指向同一对象的多个shared_ptr可以被多个线程写(包括析构)，同一个shared_ptr不能被多个线程读写。
**其他stl容器和string的线程安全等级都和内置类型一样！！！**
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
智能指针保证了引用计数是**原子操作**，然而对同一个shared_ptr指向的对象进行读写不是线程安全的。因为shared_ptr对对象读写操作大致分为两步：原始指针读写和引用计数读写，这两步并没有原子化。
