---
title: Observer（观察者）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

class Subject<< (A,#FF7700) abstract>>
{
  + notifyObservers(Object)
}
note right: 抽象主题

class ConcreteSubject
{
  + notifyObservers(Object)
}
note right: 具体主题

class Observer<< (I,#FF7700) interface>>
{
  + update(Object)
}
note right: 抽象观察者

class ConcreteObserver
{
  + update(Object)
}
note right: 具体观察者

Subject <|-- ConcreteSubject
Subject "1" o-- "0..*" Observer
Observer <|.. ConcreteObserver

{% endplantuml %}

### 意图
定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

### 适用性
* 当一个抽象模型有两个方面，其中一个方面依赖于另一个方面，将这两者封装在独立的对象中以使它们可以各自独立地改变和复用；
* 当对一个对象的改变需要同时改变其他对象，而不知道具体有多少对象有待改变时；
* 当一个对象必须通知其他对象，而它又不能假定其他对象是谁，即不希望这些对象是紧耦合的；