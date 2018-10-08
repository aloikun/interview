## 简介

LinkedList是一个实现了List接口和Deque接口的双端链表。 LinkedList底层的链表结构使它支持高效的插入和删除操作，另外它实现了Deque接口，使得LinkedList类也具有队列的特性; LinkedList不是线程安全的，如果想使LinkedList变成线程安全的，可以调用静态类Collections类中的synchronizedList方法：

```java
List list=Collections.synchronizedList(new LinkedList(...));
```

## 内部结构分析

LinkedList类中的一个**内部私有类Node**就很好理解了：

```java
private static class Node<E> {
        E item;//节点值
        Node<E> next;//前驱节点
        Node<E> prev;//后继节点

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

这个类就代表双端链表的节点Node。这个类有三个属性，分别是前驱结点，当前结点的值，后继结点。

## LinkedList源码分析

### 构造方法： 创建出来

空构造方法：



### 成员方法： 增删查改操作

add(E e) 方法: 将元素追加到链表末尾

```java
public boolean add(E e) {
    linkLast(e);
    return true;
}

/**
* 链接使e作为最后一个元素。
*/
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;//新建节点
    if (l == null) 
        first = newNode;
    else
        l.next = newNode;//指向后继元素也就是指向下一个元素
    size++;
    modCount++;
}
```

**add(int index,E e)**：在指定位置添加元素

```java
public void add(int index, E element) {
    checkPositionIndex(index); //检查索引位置是否处于[0-size]之间
    if (index == size)//添加在链表尾部
    	linkLast(element);
    else//添加在链表中间
    	linkBefore(element, node(index));
}
```

添加到链表头：

 A ->  b - > c, 在A 之前添加在， A 有三个记录的域

```java
public void addFirst(E e){
    linkFirst(e);
}
private void linkFirst(E e){
    // 先保存 链表的头部信息
    final Node<E> f = first;
    // 当前节点值为null, 前驱节点是要插入的值， 后继节点是未插入链表中的值
    final Node<E> newNode = new Node<>(null, e, f);
    first = newNode;
    if（f == null）{
        last = newNode;
    }else{
        f.prev = newNode;
    }
    size++;
    modCount++;
}
```

添加到链表尾部.

### 根据位置获取值

#### **get(int index)：**：根据指定索引返回数据

```java
public E get(int index) {
    //检查index范围是否在size之内,
    // 若是不在这个里面，则会出现IndexOutOfBoundsException（索引下表越界异常）
    checkElementIndex(index);
    //调用Node(index)去找到index对应的node然后返回它的值
    return node(index).item;
}
```

获取头结点数据方法：

**区别：** getFirst(),element(),peek(),peekFirst() 这四个获取头结点方法的区别在于对链表为空时的处理，是抛出异常还是返回null，其中**getFirst()** 和**element()** 方法将会在链表为空时，抛出异常

element()方法的内部就是使用getFirst()实现的。它们会在链表为空时，抛出NoSuchElementException **获取尾节点（index=-1）数据方法:**