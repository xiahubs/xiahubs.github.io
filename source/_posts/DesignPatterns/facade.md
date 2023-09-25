---
title: Facade （外观）模式
tags: 
  - 设计模式
date: 2023-09-24
---

{% plantuml %}
!theme sketchy-outline

abstract class Function {
  + start(): void
  + close(): void
}

class CPU {
  + start(): void
  + close(): void
}
class Disk {
  + start(): void
  + close(): void
}
class Memory {
  + start(): void
  + close(): void
}

class Computer {
  - cpu: cpu
  - disk: disk
  - memory: memory
  --
  + start(): void
  + close(): void
}

CPU .up.> Function
Disk .up.> Function
Memory .up.> Function
Computer -up-> CPU
Computer -up-> Disk
Computer -up-> Memory
{% endplantuml %}

### 意图
为子系统中的一组接口提供一个一致的界面，*Facade* 模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

### 适用性
* 要为一个复杂子系统提供一个简单接口时，子系统往往因为不断演化而变得越来越复杂。大多数模式使用时都会产生更多更小的类，这使得子系统更具有可重用性，也更容易对子系统进行定制，但也给那些不需要定制子系统的用户带来一些使用上的困难。*Facade* 可以提供一个简单的默认视图，这一视图对大多数用户来说已经足够，而那些需要更多的可定制性的用户可以越过 *Facade* 层；
* 客户程序与抽象类的实现部分之间存在着很大的依赖性。引入 *Facade* 将这个子系统与客户以及其他的子系统分离，可以提高子系统的独立性和可移植性；
* 当需要构建一个层次结构的子系统时，使用 *Facade* 模式定义子系统中每层的入口点。如果子系统之间是相互依赖的，则可以让它们仅通过*Facade* 进行通信，从而简化了它们之间的依赖关系。

### 优缺点
* 降低了子系统与客户端之间的耦合度，使得子系统的变化不会影响调用它的客户类；
* 对客户屏蔽了子系统组件，减少了客户处理的对象数目，并使得子系统使用起来更加容易；
* 降低了大型软件系统中的编译依赖性，简化了系统在不同平台之间的移植过程，因为编译一个子系统不会影响其他的子系统，也不会影响外观象；
* 不能很好地限制客户使用子系统类；
* 增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。

### 代码示例
```c#
/// 抽象功能接口
interface Function {
  void start();
  void close();
}
/// 实现子系统的功能;处理有 Facade 对象指派的任务;没有 Facade 的任何相关信息，即没有指向 Facade 的指针。
class CPU : Function {
	override void start() ==> ("启动 CPU");
	override void close() ==> ("关闭 CPU");
}
class Disk : Function {
	override void start() ==> ("启动 Disk");
	override void close() ==> ("关闭 Disk");
}
class Memory : Function {
	override void start() ==> ("启动 Memory");
	override void close() ==> ("关闭 Memory");
}

/// Facade 知道哪些子系统类负责处理请求;将客户的请求代理给适当的子系统对象。
public class Computer : Function {

	private CPU cpu;
	private Disk disk;
	private Memory memory;
  

	public Computer() {
		this.cpu = new CPU();
		this.disk = new Disk();
		this.memory = new Memory();
	}
  
	public override void start() {
		this.cpu.start();
		this.disk.start();
		this.memory.start();
	}
	public override void close() {
		this.cpu.close();
		this.disk.close();
		this.memory.close();
	}
}
```
