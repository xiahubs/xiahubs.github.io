---
title: Mediator（中介者）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

class Mediator {
  
}

class ConcreteMediator {

}
class Colleague {

}
class ConcreteColleagua1 {

}
class ConcreteColleagua2 {
  
}

Colleague "mediator" -left-> Mediator
ConcreteMediator -up-> Mediator
ConcreteColleagua1 -up-> Colleague
ConcreteColleagua2 -up-> Colleague
ConcreteMediator --> ConcreteColleagua1
ConcreteMediator --> ConcreteColleagua2

{% endplantuml %}

### 意图
用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

### 适用性
* 一组对象以定义良好但是复杂的方式进行通信，产生的相互依赖关系结构混乱且难以理解；
* 一个对象引用其他很多对象并且直接与这些对象通信，导致难以复用该对象；
* 想定制一个分布在多个类中的行为，而又不想生成太多的子类。

### 优缺点
* 单一职责原则。 你可以将多个组件间的交流抽取到同一位置， 使其更易于理解和维护；
* 开闭原则。 你无需修改实际组件就能增加新的中介者；
* 你可以减轻应用中多个组件间的耦合情况；
* 你可以更方便地复用各个组件。

---

* 一段时间后， 中介者可能会演化成为上帝对象。