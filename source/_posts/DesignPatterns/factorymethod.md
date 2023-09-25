---
title: Factory Method（工厂方法）模式
tags: 
  - 设计模式
date: 2023-09-18
---

{% plantuml %}
!theme sketchy-outline

abstract class Factory
{
  + createProduct() : Product
}

class ConcreteFactory
{
  + createProduct() : Product
}

abstract class Product
{
  + info() : void
}

class ConcreteProduct
{
  + info() : void
}

Product <|-- ConcreteProduct
Factory <|-- ConcreteFactory
ConcreteProduct <.. ConcreteFactory

{% endplantuml %}

### 意图
定义一个用于创建对象的接口，让子类决定实例化哪一个类。*Factory Method* 使一个类的实例化延迟到其子类。

### 适用性
* 当一个类不知道它所必须创建的对象的类的时候；
* 当一个类希望由它的子类来指定它所创建的对象的时候；
* 当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候。

### 代码示例
```c#
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

/// 定义接口，定义工厂方法
public interface Factory
{
  Product createProduct();
}

/// 具体子类继承Factory接口并实现创建子类
public class FactoryA : Factory
{
  override Product createProduct() => new ProductA();
}
public class FactoryB : Factory
{
  override Product createProduct() => new ProductB();
}

```