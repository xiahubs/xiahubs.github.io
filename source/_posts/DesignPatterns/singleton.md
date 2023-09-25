---
title: Singleton（单例）模式
tags: 
  - 设计模式
date: 2023-09-19
---

{% plantuml %}
!theme sketchy-outline

class Singleton
{
  + getInstance() : Singleton
  - Singleton()
}
note right: return uniqueInstance\n全局访问点

Singleton <.. Client

{% endplantuml %}

### 意图
保证一个类仅有一个实例，并提供一个访问它的全局访问点。

### 适用性
* 当类只能有一个实例而且客户可以从一个众所周知的访问点访问它时；
* 当这个唯一实例应该是通过子类化可扩展的，并且客户无须更改代码就能使用一个扩展的实例时。

### 代码示例
```c#
/// :Singleton 指定一个Instance操作，允许客户访问它的唯一实例，
class Singleton {
  private static Singleton instance = new Singleton();
  
  private Singleton() {}
  
  /// Instance 是一个类操作;负责创建它自己的唯一实例。
  public static Instance() => this.instance;
}
```