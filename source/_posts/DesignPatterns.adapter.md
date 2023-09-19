---
title: Adapter（适配器）模式
tags: 
  - 设计模式
date: 2023-09-19
---

{% plantuml %}
!theme sketchy-outline
class Client {

}
class Adapter {
  + Request()
}
note left of Adapter : SpecificRequest()
class Adaptee {
  + SpecificRequest():void
}
class Target {
  + Request():void
}

Client -right-|> Target
Adapter -right-|> Adaptee
Adapter "extend" -up-|> Target

note "类适配器结构" as Name
{% endplantuml %}

<br />
{% plantuml %}
!theme sketchy-outline
class Client {

}
class Adapter {
  + Request()
}
note left of Adapter : adaptee -> SpecificRequest()
class Adaptee {
  + SpecificRequest():void
}
class Target {
  + Request():void
}

Client -right-|> Target
Adapter -right-|> Adaptee
Adapter "adaptee" -up-|> Target

note "对象适配器结构" as Name
{% endplantuml %}

### 意图
将一个类的接口转换成客户希望的另外一个接口。Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

### 结构
类适配器使用多重继承对一个接口与另一个接口进行匹配
对象适配器依赖于对象组合

### 适用性
* 想使用一个已经存在的类，而它的接口不符合要求；
* 想创建一个可以服用的类，该类可以与其他不相关的类或不可预见的类(即那些接口可能不一定兼容的类)协同工作；
* (仅适用于对象Adapter)想使用一个已经存在的子类，但是不可能对每一个都进行子类化以匹配它们的接口。对象适配器可以适配它的父类接口。

### 代码示例
```c#
/// 定义Client使用的与特定领域相关的接口。
class Target {
  public string Request() => "普通请求";
}

/// 一个已经存在的接口，这个接口不满足使用序曲，需要适配。
class Adaptee {
  public string SpecificRequest() => "特殊请求";
}

/// Adapter 对 Adaptee 的接口与Target接口进行适配。
class Adapter : Target {
  private Adaptee adaptee = new Adaptee();
  
  /// 重写 Request() 调用适配接口
  override string Request() => adaptee.SpecificRequest();
}
```