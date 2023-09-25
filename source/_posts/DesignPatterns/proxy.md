---
title: Proxy（代理）模式
tags: 
  - 设计模式
date: 2023-09-24
---

{% plantuml %}
!theme sketchy-outline

class Client

abstract class Subject
{
	+ visit()
}
note right: 抽象主题类

class RealSubject
{
	+ visit()
}
note right: 真实主题类

class ProxySubject
{
	- mSubject : RealSubject
	+ visit()
}
note right: 代理类

Subject <.left. Client
Subject <|-- RealSubject
Subject <|-- ProxySubject
RealSubject <-- ProxySubject

{% endplantuml %}

### 意图
代理控制着对于原对象的访问， 并允许在将请求提交给对象前后进行一些处理。

### 适用性
* *Proxy* 模式适用于在需要比较通用和复杂的对象指针代替简单的指针的时候，常见情况有:
* 远程代理( *Remote Proxy* )为一个对象在不同地址空间提供局部代表。
* 虚代理( *Virtual Proxy* )根据需要创建开销很大的对象。
* 保护代理( *Protection Proxy* )控制对原始对象的访问，用于对象应该有不同的访问权限的时候。
* 智能引用( *Smart Reference* )取代了简单的指针，它在访问对象时执行一些附加操作。
* 典型用途包括:对指向实际对象的引用计数，这样当该对象没有引用时，可以被自动释放;当第一次引用一个持久对象时，将它装入内存;在访问一个实际对象前，检查是否已经锁定了它，以确保其他对象不能改变它。

### 优缺点
* 你可以在客户端毫无察觉的情况下控制服务对象；
* 如果客户端对服务对象的生命周期没有特殊要求，你可以对生命周期进行管理；
* 即使服务对象还未准备好或不存在，代理也可以正常工作；
* 开闭原则：你可以在不对服务或客户端做出修改的情况下创建新代理。

---

* 代码可能会变得复杂，因为需要额外新建类；
* 服务响应可能会延迟。

### 代码示例
```c#
interface Subject {
	public string buy();
}

class Proxy : Subject {
	protected RealSubject realSubject;

	public Proxy(RealSubject realSubject) {
		this.realSubject = realSubject;
	}
	
	public override string buy() {
		// prev-operation
		...
		this.realSubject.buy();
		// next-operation
		...
	}
}

class RealSubject : Subject {

	public override string buy() => "payment";
}

```
