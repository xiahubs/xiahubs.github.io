---
title: 抽象工厂模式
tags: 
  - 设计模式
date: 2023-09-18
---

{% plantuml %}
!theme sketchy-outline
interface AbstractFactory << interface >> {
  + CreaterProductA(): void
  + CreaterProductB(): void
}
interface ConcreteFactory1 << interface >> {
  + CreateProductA(): void
  + CreateProduct2(): void
}
interface ConcreteFactory2 << interface >> {
  + CreateProductA(): void
  + CreateProduct2(): void
}

abstract AbstractProductA << interface >> {
}
class ProductA2 {

}
class ProductA1 {

}

abstract AbstractProductB << interface >> {
}
class ProductB2 {

}
class ProductB1 {

}
class client{}

AbstractFactory <|-- ConcreteFactory1
AbstractFactory <|-- ConcreteFactory2

AbstractProductA <|-right- ProductA1
AbstractProductA <|-right- ProductA2

AbstractProductB <|-- ProductB1
AbstractProductB <|-- ProductB2

AbstractFactory <|-up- client
AbstractProductA <|-- client
AbstractProductB <|-- client

ConcreteFactory1 <|.. ProductA2
ConcreteFactory1 <|.. ProductA1

ConcreteFactory2 <|.. ProductB2
ConcreteFactory2 <|.. ProductB1
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