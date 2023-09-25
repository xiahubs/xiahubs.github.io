---
title: Memento（备忘录）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

class Originator
{
  + restore(Memento)
  + createMemento(): Memento
}
note right: 负责创建备忘录

class Memento
{
  - mState
  + setState(int)
  + getState():int
}
note right: 备忘录

class Caretaker
{
  - mMemento:Memento
  + restoreMemento(): Memento
  + storeMemento(Memento): void
}
note right:负责存储备忘录

Originator ..> Memento
Memento --o Caretaker

{% endplantuml %}

### 意图
在不破坏封装性的前提下捕获一个对象的内部状态，并在对象之外保存这个状态。这样以后就可以将对象恢复到原先保存的状态。

### 适用性
* 必须保存一个对象在某一个时刻的(部分)状态，这样以后需要时它才能恢复到先前的状态；
* 如果一个用接口来让其他对象直接得到这些状态，将会暴露对象的实现细节并破坏对象的封装性。