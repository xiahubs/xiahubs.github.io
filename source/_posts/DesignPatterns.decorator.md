---
title: Decorator（装饰器）模式
tags: 
  - 设计模式
date: 2023-09-19
---

{% plantuml %}
!theme sketchy-outline
class Component {
  + Operation()
}
class ConcreteComponent {
  + Operation()
}
class Decorator {
  + Operation()
}
note left of Decorator : component -> Operation()
class ConcreteDecoratorA {
  + Operation()
  ---
  addedState
}
class ConcreteDecoratorB {
  + Operation()
  + AddedBehavior()
}
note right of ConcreteDecoratorB : Decorator:: Operation()\r\nAddedBehavior()

ConcreteComponent -up-> Component
Decorator "component“ -up-> Component
ConcreteDecoratorA -up-> Decorator
ConcreteDecoratorB -up-> Decorator
{% endplantuml %}

### 意图
动态的给一个对象添加一些额外的职责。就增加功能而言，Decorator模式比生成子类更加灵活。

### 适用性
* 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
* 处理那些可以撤销的职责。
* 当不能采用生成子类的方式进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是，由于类定义被隐藏，或类定义不能用于生成子类。
