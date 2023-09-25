---
title: Abstract Factory（抽象工厂）模式
tags: 
  - 设计模式
date: 2023-09-18
---

{% plantuml %}
!theme sketchy-outline
 
abstract class AbstractProductA
{
  + info() : void
}

abstract class AbstractProductB
{
  + info() : void
}

class ConcreteProductA1
{
  + info() : void
}

class ConcreteProductA2
{
  + info() : void
}

class ConcreteProductB1
{
  + info() : void
}

class ConcreteProductB2
{
  + info() : void
}

abstract class AbstractFactory
{
  + createProductA() : AbstractProductA
  + createProductB() : AbstractProductB
}

class ConcreteFactory1
{
  + createProductA() : AbstractProductA
  + createProductB() : AbstractProductB
}

class ConcreteFactory2
{
  + createProductA() : AbstractProductA
  + createProductB() : AbstractProductB
}

AbstractProductA <|-- ConcreteProductA1
AbstractProductA <|-- ConcreteProductA2
AbstractProductB <|-- ConcreteProductB1
AbstractProductB <|-- ConcreteProductB2
AbstractFactory <|-- ConcreteFactory1
AbstractFactory <|-- ConcreteFactory2
ConcreteFactory1 ..> ConcreteProductA1
ConcreteFactory1 ..> ConcreteProductB1
ConcreteFactory2 ..> ConcreteProductA2
ConcreteFactory2 ..> ConcreteProductB2

{% endplantuml %}

### 意图
提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

### 适用性
* 一个系统要独立于它的产品的创建、组合和表示时；
* 一个系统要由多个产品系列中的一个来配置时；
* 当要强调一系列相关的产品对象的设计以便进行联合使用时；
* 当提供一个产品类库，只想显示它们的接口而不是实现时。

### 代码示例
```c#
// 抽象接口，定义产品
public interface AbstractFactory
{
  public ProductA ConcreteFactory1();
  public ProductB ConcreteFactory2();
}

/// 具体产品继承AbstractFactory，并实现其中方法
public class Factory1 : AbstractFactory
{
  override ProductA ConcreteFactory1() => return ProductA1();
  override ProductA ConcreteFactory2() => return ProductB1();
}
/// 具体产品继承AbstractFactory ，并实现其中方法
public class Factory1 : AbstractFactory
{
  override ProductA ConcreteFactory1() => return ProductA2();
  override ProductB ConcreteFactory2() => return ProductB2();
}

// 抽象接口，定义产品
public interface Product
{
   void info();
}

/// 具体产品继承Product ，并实现其中方法
public class ProductA : Product
{
  override void info();
}
public class ProductB : Product
{
  override void info();
}

```