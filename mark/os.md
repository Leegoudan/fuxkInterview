# 分布式进程通信
## 事件模型
  - jvm 封装nio，模拟同步- 非阻塞模型 [reactor](###reactor)
  - .net 封装异步模型 [proactor](###proactor)
  - asio 封装异步模型 [proactor](###proactor)
  - node.js
  - python
  - go
  - erlang
  - rust
### reactor
### proactor
## 并发模型
### one loop per thread
### actor
### csp
## 初始化模型
### acceptor
### connector
### service configurator
## 混合模型
# 磁盘I/O
# 网络I/O
  - 同步 - 阻塞
  - 同步 - 非阻塞
  - 多路复用
    - select
    - poll
    - epoll
  - 异步 windows
    - iocp
# 多线程
## 锁
# 状态
# 数据库
  ## 数据库索引
  - B+树
  - 跳表
    - 不用同步锁
  - Bw树
  - ART（自适应树）
  - Masstree
    - B+树和Radix树的混合体。