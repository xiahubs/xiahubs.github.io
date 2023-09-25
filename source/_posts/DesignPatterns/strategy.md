---
title: Strategy（策略）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

interface Strategy
{
  + ContextInterface()
}

class ConcreteStrategyA
{
  + algorithmInterface()
}

class ConcreteStrategyB
{
  + algorithmInterface()
}

class ConcreteStrategyC
{
  + algorithmInterface()
}
class Context
{
  + setStrategy(Strategy)
  + algorithmInterface()
}

Context "Strategy" o-- Strategy
Strategy <|.. ConcreteStrategyA
Strategy <|.. ConcreteStrategyB
Strategy <|.. ConcreteStrategyC

{% endplantuml %}

### 意图
定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。此模式使得算法可以独立于使用它们的客户而变化。

### 适用性
* 许多相关的类仅仅是行为有异。“策略”提供了一种用多个行为中的一个行为来配置一个类的方法；
* 需要使用一个算法的不同变体例如，定义一些反映不同空间的空间/时间权衡的算法。当这些变体实现为一个算法的类层次时，可以使用策略模式；
* 算法使用客户不应该知道的数据。可使用策略模式以避免暴露复杂的、与算法相关的数据结构；
* 一个类定义了多种行为，并且这些行为在这个类的操作中以多个条件语句的形式出现，将相关的条件分支移入它们各自的 Strategy类中，以代替这些条件语句。