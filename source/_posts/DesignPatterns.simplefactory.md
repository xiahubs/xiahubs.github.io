---
title: 简单工厂模式
tags: 
  - 设计模式
date: 2023-09-15
---

{% plantuml %}
!theme sketchy-outline
interface Product << interface >> {
  + info(): void
}

class ProductA {
  + info(): void
}

class ProductB {
  + info(): void
}

class Factory {
  + createProduct(): Product
}

Product <|-- ProductA
Product <|-- ProductB
ProductA <|.. Factory
ProductB <|.. Factory
{% endplantuml %}

简单工厂模式属于创建型模式，但不属于23种设计模式之一。

定义：定义一个工厂类，它可以根据参数的不同返回不同类的实例，被创建的实例通常都具有共同的父类

在简单工厂模式中用于被创建实例的方法通常为静态(*static*)方法,因此简单工厂模式又被成为静态工厂方法(*static Factory Method*)

> 需要什么产品就传入产品对应的参数，就可以获取所需要的产品对象，而无需知道其实现过程

**三类角色**
工厂(核心)：负责实现创建所有产品的内部逻辑，
工厂类可以被外界直接调用，创建所需对象

抽象产品：工厂类所创建的所有对象的父类，
封装了产品对象的公共方法，所有的具体产品为其子类对象

具体产品：简单工厂模式的创建目标，
所有被创建的对象都是某个具体类的实例。它要实现抽象产品中声明的抽象方法
