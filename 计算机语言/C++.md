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

# 2. 红黑树(stl的map,set,multi_map, multi_set底层实现)

*（问题来源：http://www.cppblog.com/Solstice/archive/2013/01/20/197422.html）*
1. 为什么 rb_tree 没有包含 allocator 成员？
这是stl的空基类优化。

2. 为什么 iterator 可以 pass-by-value？
属于stl问题，《Effective C++》item20.Prefer pass-by-reference-to-const to pass-by-value:内置类型、iterator、仿函数建议pass-by-value。

3. 为什么 header 要有 left 成员，并指向 left most 节点？
保证begin()是O(1)。

4. 为什么 header 要有 right 成员，并指向 right most 节点？
硕大分析过，理论上保证倒插元素的时候，时间复杂度为O(n)，实际上并没有什么卵用。

5. 为什么 header 要有 color 成员，并且固定是红色？
区别于根节点(根节点一定是黑的)。当rb_tree是空时，即只有一个header数据时，可以用(node_->color_ == kRed && node_->parent_->parent_ == node_) 判断。只有node_->parent_->parent_ == node_的话无法区分是有根节点还是只有header。 

6. 为什么要分为 rb_tree_node 和 rb_tree_node_base 两层结构，引入 node 基类的目的是什么？
解耦：把树的操作从节点类分离开来，减少模板展开引起的代码膨胀(rb_tree_node已经需要传入具体元素类型)。同时因为分离出来，iterator可以直接使用base指针来进行树的操作。

7. 为什么 iterator 的递增递减是分摊（amortized）常数时间？
硕大给了一个很简单的证明：1棵树有n个节点，对应只有n-1条边，遍历完所有节点，就是每条边都走两边，即走了2(n-1)次。所以从一个节点到下一个节点的均摊时间为常数2。
为什么是均摊？因为从一个节点到下一个节点，可能需要访问两遍树高h次(2h次，即左子树最右叶子节点到右子树最左叶子节点)。
实际上也很容易想，因为不是所有节点iterator都可能要对数时间的递增递减，只有对数个节点iterator需要对数时间的递增递减，那么总体来说遍历一棵树的时间还是趋近O(n)的。
