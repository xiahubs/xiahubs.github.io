---
title: Iterator（迭代器）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

class Client {
  
}

class Aggregate {
  + CreateIterator()
}
class Iterator {
  + First()
  + Next()
  + isDone()
  + CurrentlItem()
}

class ConcreteAggregate {
  + CreatetIterator()
}

class ConcreteIterator {

}

Client -left-> Aggregate
Client -right-> Iterator
ConcreteAggregate -up-> Aggregate
ConcreteIterator -up-> Iterator

ConcreteAggregate .right.> ConcreteIterator
ConcreteAggregate <-right- ConcreteIterator
{% endplantuml %}

### 意图
提供一种方法顺序访问一个聚合对象中的各个元素，且不需要暴露该对象的内部表示。

### 适用性
* 访问一个聚合对象的内容而无须暴露它的内部表示；
* 支持对聚合对象的多种遍历；
* 为遍历不同的聚合结构提供一个统一的接口。

### 优缺点
* 单一职责原则。 通过将体积庞大的遍历算法代码抽取为独立的类，你可对客户端代码和集合进行整理；
* 开闭原则。 你可实现新型的集合和迭代器并将其传递给现有代码，无需修改现有代码；
* 你可以并行遍历同一集合，因为每个迭代器对象都包含其自身的遍历状态；
* 相似的，你可以暂停遍历并在需要时继续。

---

* 如果你的程序只与简单的集合进行交互，应用该模式可能会矫枉过正；
* 对于某些特殊集合，使用迭代器可能比直接遍历的效率低。

### 代码示例
```c#
interface Iterator {
  public bool hasNext();
  public Object next();
}

class BookIterator : Iterator {
  private int index;
  private BookAggregate bookaggregate;

  public BookIterator(BookAggregate bookaggregate) {
    this.index = 0;
    this.bookaggregate = bookaggregate;
  }

  override bool hasNext() => index < this.bookaggregate.getSize() ? true : false;

  override Object next() => this.bookaggregate.get(index++);
}

interface Aggregate {
  public Iterator CreateIterator();
}

class BookAggregate : Aggregate {
  private List<Book> list = new ArrayList<Book>();
  
  public void Add(Book book) {
    this.list.Add(book);
  }
  public Object get(int index) => this.list[index];
  public int getSize() => list.Count();

  public override Iterator CreateIterator() => return new BookIterator(this);
}
```
