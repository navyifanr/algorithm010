## 学习笔记

### 学习心得

太久没刷算法了，为准备面试重新刷起。不过有点动力不足，所以临时插班，报了课程。自测时，成绩才刚及格，真是一言难尽。老师教的几个学习方法有点刷新我的想法，也启发不少

- 不要死磕(死磕太浪费时间了，特别是对于面向面试的刷算法)；
- 五毒神掌(以前刷题只是想AC, 过了就行，而且只刷一遍，但其实"死记硬背"也是一种高效方法，而且只做一遍确实效果不大，现在发现才隔几天，之前做的有时候都忘记了)
- 多看高质量、高票解题；


[主定理](https://zh.wikipedia.org/zh-hans/%E4%B8%BB%E5%AE%9A%E7%90%86)
[数据结构和算法总览](http://naotu.baidu.com/file/2eef270e9d86ba6d8401aa9bf9cdcd05?token=2960ac11c2cf3948)

数组问题可能的解题思路：

- 暴力遍历法
- 哈希存储
- 双指针法
  注意指针位置(同时在左边, 同时在左边 或 一左一右)，移动方向和什么时候移

链表问题：
- 迭代法
- 快慢指针

注意区分创建节点的两种方式：new新节点和直接通过老节点赋值；**(直接赋值后续新节点修改会影响原节点)**

注意节点交换的方法，如[24]两两交换链表中节点
```java
public ListNode swapPairs(ListNode head) {
    ListNode fakeHead = new ListNode(-1);
    fakeHead.next = head;
    ListNode preNode = fakeHead;
    while (head != null && head.next != null) {
        ListNode firstNode = head;
        ListNode secondNode = head.next;

        //交换节点
        preNode.next = secondNode;
        firstNode.next = secondNode.next;
        secondNode.next = firstNode;

        preNode = firstNode;
        head = firstNode.next;
    }
    return fakeHead.next;
}
```


### 作业

代码：见 `Assignments` 目录


分析 Queue 和 Priority Queue 的源码:

1. Queue(队列) 是继承自 Collection 接口的接口，是一种先进先出的线性表；
```java
public interface Queue<E> extends Collection<E> {
    boolean add(E e);     //插入元素，容量限制插入失败时会抛异常
    boolean offer(E e);   //插入元素，容量限制插入失败时会返回 false
    E remove();           //移除队列头部元素，队列为空时调用会抛异常 NoSuchElementException
    E poll();             //移除队列头部元素，队列为空时调用返回 null
    E element();          //获取队列头部元素，队列为空时调用会抛异常 NoSuchElementException
    E peek();             //获取队列头部元素，队列为空时调用返回 null
}
```

2. Priority Queue 优先队列

- 继承自 AbstractQueue 类(内部实现 Queue 接口)，是一个基于优先级堆的无边界队列(基于数组实现的小顶堆)；
- 通过 Comparable 或 Comparator 实现排序；
- 不允许添加 null 元素，不支持添加不可比较的对象，否则会出现 ClassCastException 异常；
- 虽然是无边界的，但有指定的容量大小，添加元素时，超过容量会自动增容；
- 不是同步的，线程不安全，具有线程安全的优先队列使用 PriorityBlockingQueue 类；
- 提供的入队出队方法(offer, poll, remove(), add) 时间复杂度为O(log n)； remove(object) 和 contain(object) 时间复杂度 O(n)；检索方法 peek, element, size 时间复杂度为 O(1)

创建时，可以指定初始容量和比较器
```java
public PriorityQueue(int initialCapacity) {
    this(initialCapacity, null);
}

public PriorityQueue(Comparator<? super E> comparator) {
    this(DEFAULT_INITIAL_CAPACITY, comparator);
}

public PriorityQueue(int initialCapacity, Comparator<? super E> comparator) {
    //……
}
```

自动增容：当容量较小时，直接加倍，否则增加 50%
```java
private void grow(int minCapacity) {
    int oldCapacity = queue.length;
    // Double size if small; else grow by 50%
    int newCapacity = oldCapacity + ((oldCapacity < 64) ?
                                     (oldCapacity + 2) :
                                     (oldCapacity >> 1));
    // overflow-conscious code
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    queue = Arrays.copyOf(queue, newCapacity);
}
```


