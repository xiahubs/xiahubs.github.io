---
title: Prototype（原型）模式
tags: 
  - 设计模式
date: 2023-09-19
---

{% plantuml %}
!theme sketchy-outline
class Client {
  + Operation()
}
note right of Client
p = prototype->Clone()
end note

interface Prototype {
  + Clone()
}

class ConcretePrototype1 {
  + Clone()
}
note bottom of ConcretePrototype1: return copy of self
class ConcretePrototype2 {
  + Clone()
}
note bottom of ConcretePrototype2: return copy of self

Client "prototype" o-right-> Prototype
ConcretePrototype1 -up-> Prototype
ConcretePrototype2 -up-> Prototype
{% endplantuml %}

### 意图
用原型实例指定创建对象的种类，并且通过复制这些原型创建新的对象。

### 适用性
* 当一个系统应该独立于它的产品创建、构成和表示时；
* 当要实例化的类是在运行时刻指定时，例如，通过动态装载；
* 为了避免创建一个与产品类层次平行的工厂类层次时；
* 当一个类的实例只能有几个不同状态组合中的一种时。建立相应数目的原型并克隆它们，可能比每次用合适的状态手工实例化该类更方便一些。

### 代码示例
```c#
/// 声明一个复制自身的接口。
interface Prototype {
  public Object Clone();
}

/// 实现一个复制自身的操作。
class Product : Prototyp {
  private int id;
  private double price;
  
  public Product() {}
  public Product(int id, double price) {
    this.id = id;
    this.price = price;
  }

  public int Id {
    get { return this.id; }
  }
  public double Price {
    get { return this.price; }
  }

  /// 复制具体属性并返回
  override Object Clone() {
    Product objects = new Prodcut();
    objects.id = this.id;
    objects.price = this.price;
    
    return objects;
  };
}
```