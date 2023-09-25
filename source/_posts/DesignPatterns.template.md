---
title: Template（模板方法）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

class AbsTemplate<< (A,#FF7700) abstract>> {
  # stepOne(): void
  # stepTwo(): void
  # stepThree(): void
  + execute(): void
}
note right: 定义算法框架的抽象类

class ConcreteImplA

class ConcreteImplB


class ConcreteImplC

AbsTemplate <|-- ConcreteImplA
AbsTemplate <|-- ConcreteImplB
AbsTemplate <|-- ConcreteImplC

{% endplantuml %}

### 意图
定义一个操作中的算法骨架，而将一些步骤延迟到子类中。*Template Method* 使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

### 适用性
* 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现；
* 各子类中公共的行为应被提取出来并集中到一个公共父类中，以避免代码重复；
* 控制子类扩展。模板方法旨在特定点调用“*hook* ”操作(默认的行为，子类可以在必要时进行重定义扩展)，这就只允许在这些点进行扩展，