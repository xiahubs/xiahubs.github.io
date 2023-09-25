---
title: Builder（生成器）模式
tags: 
  - 设计模式
date: 2023-09-18
---

{% plantuml %}
!theme sketchy-outline

abstract class Builder
{
  + buildPartA() : void
  + buildPartB() : void
  + buildPartC() : void
}
note right: 抽象Builder类

class ConcreteBuilder
{
  + buildPartA() : void
  + buildPartB() : void
  + buildPartC() : void
}
note right: 具体Builder类

class Director
{
  + construct()
}
note right:统一组装过程

abstract class Product
note right:产品的抽象类

Director o-- Builder
Builder <|-- ConcreteBuilder
Product <.. ConcreteBuilder:<<use>>
{% endplantuml %}

### 意图
将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

### 适用性
* 当创建复杂对象的算法应该独立于该对象的组成部分以及它们的装配方式时；
* 当构造过程必须允许被构造的对象有不同的表示时。
### 代码示例
```c#
/// 构造一个使用 Builder 接口的对象
class Director {
  public void Construct(Builder builder) {
    builder.BuildPart();;
  }
}

/// 实现Builder的接口以构造和装配该产品的各个部件，定义并明确它所创建的表示，提供一个检索产品的接口。
/// 创建该产品的内部表示并定义它的装配过程。包含定义组成组件的类，包括将这些组件装配成最终产品的接口。
class Builder2 : Builder
{
  Product product = new Product();

  override void BuildParts() {
    product.Add("F");
    product.Add("G");
    product.Add("H");
  }
  override Product getResult() => this.product;
}
class Builder1 : Builder
{
  Product product = new Product();

  override void BuildParts() {
    product.Add("A");
    product.Add("B");
    product.Add("C");
    product.Add("D");
    product.Add("E");
  }
  override Product getResult() => this.product;
}

/// 为创建一个Product 对象的各个部件指定抽象接口。
abstract class Builder
{
  public abstract void BuildParts();
  public abstract Product getResult();
}

// 表示被构造的复杂对象
class Product
{
  List<string> parts = new List<string>();

  public void Add(string part) => this.parts.Add(part);

  public void show() 
  {
    for(var item in this.parts) { Console.Write($@"{item}  "); Console.WriteLine(); }
  }
}

```