---
title: Visitor（访问者）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

class Visitor<< (I,#FF7700) abstract>>
{
  + visitElementA(ElementA): void
  + visitElementB(ElementB): void
}
note top of Visitor: 访问者接口或抽象类

class ConcreteVisitorA
{
  + visitElementA(ElementA): void
  + visitElementB(ElementB): void
}

class ConcreteVisitorB
{
  + visitElementA(ElementA): void
  + visitElementB(ElementB): void
}

class Element<< (I,#FF7700) abstract>>
{
  + accept(Visitor)
}
note top of Element: 元素接口或抽象类

class ElementA
{
  + accept(Visitor)
  + operationA()
}

class ElementB
{
  + accept(Visitor)
  + operationB()
}

class ObjectStructure
note bottom of ObjectStructure: 管理元素集合的对象结构

class Client

Visitor <|-- ConcreteVisitorA
Visitor <|-- ConcreteVisitorB
Element <|-- ElementA
Element <|-- ElementB
Element --o ObjectStructure
Client ..> Visitor:<<use>>
Client ..> ObjectStructure:<<use>>

{% endplantuml %}

### 意图
表示一个作用于某对象结构中的各元素的操作。它允许在不改变各元素的类的前提下定义作用于这些元素的新操作。

### 适用性
一个对象结构包含很多类对象，它们有不同的接口，而用户想对这些对象实施一些依赖于其具体类的操作。
需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而又想要避免这些操作“污染”这些对象的类。*Visitor* 使得用户可以将相关的操作集中起来定义在一个类中。当该对象结构被很多应用共享时，用 *Visitor* 模式让每个应用仅包含需要用到的操作。
定义对象结构的类很少改变，但经常需要在此结构上定义新的操作。改变对象结构类需要重定义对所有访问者的接口，这可能需要很大的代价。如果对象结构类经常改变，那么可能还是在这些类中定义这些操作较好。