---
title: Command（命令）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

class Client {

}
class Receiver {
  + Action()
}
class Invoker {

}

class Command {
  + Execute()
}
class ConcreteCommand {
  + Execute()
  --
  + state
}
note right of ConcreteCommand : receiver -> Action();

Client --> Receiver
Client ..> ConcreteCommand

Invoker o-right-> Command
ConcreteCommand --> Command

Receiver "receiver" -right-> ConcreteCommand
{% endplantuml %}

### 意图
将一个请求封装为一个对象，从而使得可以用不同的请求对客户进行参数化;对请求排队或记录请求日志，以及支持可撤销的操作。

### 适用性
* 抽象出待执行的动作以参数化某对象。*Command* 模式是过程语言中的回调(*Callback*)机制的一个面向对象的替代品。
* 在不同的时刻指定、排列和执行请求。一个 *Command* 对象可以有一个与初始请求无关的生存期。如果一个请求的接收者可用一种与地址空间无关的方式表达，那么就可以将负责该请求的命令对象传递给另一个不同的进程并在那儿实现该请求。
* 支持取消操作。*Command* 的 *Execute* 操作可在实施操作前将状态存储起来，在取消操作时这个状态用来消除该操作的影响。*Command* 接口必须添加一个 *Unexecute* 操作，该操作取消上一次 *Execute* 调用的效果。执行的命令被存储在一个历史列表中。可通过向后和向前遍历这一列表并分别调用 *Unexecute* 和*Execute* 来实现重数不限的“取消”和“重做”。
* 支持修改日志。这样当系统崩溃时，这些修改可以被重做一遍。在 *Command* 接口中添加装载操作和存储操作，可以用来保持变动的一个一致的修改日志。从崩溃中恢复的过程包括从磁盘中重新读入记录下来的命令并用 *Execute* 操作重新执行它们。
* 用构建在原语操作上的高层操作构造一个系统。这样一种结构在支持事务(*Transaction*)的信息系统中很常见。*Command* 模式提供了对事务进行建模的方法。*Command* 有一个公共接口，使得可以用同一种方式调用所有的事务，同时使用该模式也易于添加新事务以扩展系统。

### 优缺点
* 单一职责原则。 你可以解耦触发和执行操作的类。
* 开闭原则。 你可以在不修改已有客户端代码的情况下在程序中创建新的命令。
* 你可以实现撤销和恢复功能。
* 你可以实现操作的延迟执行。
* 你可以将一组简单命令组合成一个复杂命令。

---

* 代码可能会变得更加复杂， 因为你在发送者和接收者之间增加了一个全新的层次。

### 代码示例
```c#
/// 命令接口
interface Command {
  public string Execute();
}

class OnCommand : Command {
  private Tv tv;

  // 开机行为
  public OnCommand (Tv tv) {
    this.tv = tv;
  }

  public override string Execute() => this.tv.OnAction();
}

class OffCommand : Command {
  private Tv tv;

  // 关机行为
  public OffCommand (Tv tv) {
    this.tv = tv;
  }

  public override string Execute() => this.tv.OffCommand();
}

class Tv {
  // 开机行为
  public string OnAction() => "OnAction";

  // 关机行为
  public string OffAction() => "OffAction";
}

class Invoker {
  private Command command;

  // 设置请求者的请求的命令
  public void setCommand(Command command) {
    this.command = command;
  }

  // 调用
  public string call() => this.command.Execute();
}
```
