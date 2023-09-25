---
title: Chain of Responsibility（责任链）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

abstract class Handler
{
  # successor() : Handler
  + handleRequest(String) : void
}
note left: 抽象处理者

class ConcreteHandler1
{
  + handleRequest(String) : void
}

class ConcreteHandler2
{
  + handleRequest(String) : void
}

Handler o-- Handler:0..1
Handler <|-- ConcreteHandler1
Handler <|-- ConcreteHandler2
Client ..> Handler

{% endplantuml %}

### 意图
使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。

### 适用性
* 有多个的对象可以处理一个请求，哪个对象处理该请求运行时刻自动确定；
* 想在不明确指定接收者的情况下向多个对象中的一个提交一个请求；
* 可处理一个请求的对象集合应被动态指定。

### 优缺点


### 代码示例
```c#
abstract class Handler {
  protected Handler next;
  
  public void setNext(Handler next) {
    this.next = next;
  }

  public abstract string HandlerRequest(int request);
}

class Instructor : Handler {
  public override string HandlerRequest(int request) {
    if(request <= 7) => "approved by instructor";
    else  return next != null ? next.HandlerRequest(request) : "unable to approve"; 
  }
}
class Dean : Handler {
  public override string HandlerRequest(int request) {
    if(request <= 20) => "approved by dean";
    else  return next != null ? next.HandlerRequest(request) : "unable to approve"; 
  }
}
class Rector : Handler {
  public override string HandlerRequest(int request) {
    if(request <= 30) => "approved by rector";
    else  return next != null ? next.HandlerRequest(request) : "unable to approve"; 
  }
}
```
