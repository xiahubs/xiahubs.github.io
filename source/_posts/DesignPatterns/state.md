---
title: State（状态）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

interface State
{
  + Handle(): void
}
note right: 抽象状态类或状态接口

class ConcreteStateA
{
  + Handle(): void
}

class ConcreteStateB
{
  + Handle(): void
}

class ConcreteStateC
{
  + Handle(): void
}


class Context
{
  + Request(): void
}

Context -- State
State <|.. ConcreteStateA
State <|.. ConcreteStateB
State <|.. ConcreteStateC

{% endplantuml %}

### 意图
允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类。

### 适用性
* 一个对象的行为决定于它的状态，并且它必须在运行时刻根据状态改变它的行为。
* 一个操作中含有庞大的多分支的条件语句，且这些分支依赖于该对象的状态。这个状态常用一个或多个枚举常量表示。通常，有多个操作包含这一相同的条件结构。*State* 模式将每一个条件分支放入一个独立的类中。这使得开发者可以根据对象自身的情况将对象的状态作为一个对象，这一对象可以不依赖于其他对象独立变化。