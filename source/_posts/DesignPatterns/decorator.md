---
title: Decorator（装饰器）模式
tags: 
    - 设计模式
date: 2023-09-19
---

{% plantuml %}
!theme sketchy-outline

abstract class Component
{
  + operate() : void
}
note top of Component: 抽象组件

class ConcreteComponent
{
  + operate() : void
}
note top of ConcreteComponent: 组件具体实现类

abstract class Decorator
{
  - component:Component
  + Decorator(Component)
  + operate() : void
}
note left: 抽象类装饰者

class ConreteDecoratorA
{
  + ConreteDecoratorA(Component)
  + operate() : void
  + operateA() : void
  + operateB() : void
}

class ConreteDecoratorB
{
  + ConreteDecoratorB(Component)
  + operate() : void
  + operateA() : void
  + operateB() : void
}

Component <|-- ConcreteComponent
Component <|-- Decorator
Component <--o Decorator
ConreteDecoratorA --|> Decorator
ConreteDecoratorB --|> Decorator

{% endplantuml %}

### 意图
动态的给一个对象添加一些额外的职责。就增加功能而言，Decorator模式比生成子类更加灵活。

### 适用性
* 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
* 处理那些可以撤销的职责。
* 当不能采用生成子类的方式进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是，由于类定义被隐藏，或类定义不能用于生成子类。
