---
title: Bridge（桥接）模式
tags: 
  - 设计模式
date: 2023-09-19
---

{% plantuml %}
!theme sketchy-outline
class Client {

}
class Abstraction {
  + Operation()
}
note right of Abstraction : imp => imp -> OperationImp();
class RefinedAbstraction {

}
class Implementor {
  + OperationImp()
}
class ConcreteImplementorA {
  + OperationImp()
}
class ConcreteImplementorB {
  + OperationImp()
}
Client -right-> Abstraction
Abstraction "extend" o-right-> Implementor
RefinedAbstraction -up-> Abstraction
ConcreteImplementorA -up-> Implementor
ConcreteImplementorB -up-> Implementor


{% endplantuml %}

### 意图
将抽象部分与其实现部分分离，使它们都可以独立的变化。

### 适用性
* 不希望在抽象和它的实现部分之间有一个固定的绑定关系。例如，这种情况可能是因为，在程序运行时刻实现部分应可以被选择或者切换。
* 类的抽象以及它的实现都应该可以通过生成子类的方法加以扩充。这是Bridge模式使得开发者可以对不同的抽象接口和实现部分进行组合，并分别对它们进行扩充。
* 对一个抽象的实现部分的修改应对客户不产生影响，即客户代码不必重新编译。
* (C++)想对客户完全隐藏抽象的实现部分。
* 有许多类要生成的类层次结构。
* 想在多个对象间共享实现(可能使用引用计数)，但同时要求客户并不知道这一点。

### 代码示例
```c#
/// 视频类型也可以不断的扩展 , 如 : H264 , H265 , MPEG 等;
/// 定义基于这些基本操作的接口。
interface Vedio {
    Vedio openVedio();

    string VedioInfo();
}

/// 实现Implementor接口并定义它的具体实现。
public class FlvVedio : Vedio {
  override Vedio openVedio() => new FilVideo();

  override Vedio VedioInfo() => "当前视频格式是 FLV";
}
public class Mp4Vedio : Vedio {
  override Vedio openVedio() => new Mp4Vedio();
  
  override Vedio VedioInfo() => "当前视频格式是 FLV";
}


/// 平台可以不断的扩展 , 如 : Windows , iOS , MAC , 嵌入式平台;
public abstract class Platform {
  // 定义抽象类的接口，维护一个指向Implementor类型对象的指针。
  // 在 Platform 中通过组合方式关联 Vedio
  protected Vedio video;

  public Platform(Vedio video) {
    this.video = video;
  }
  
  abstract Vedio openVedio();
}

/// 扩展平台
public class LinuxPlatform : Platform {
  public LinuxPlatform(Vedio video) : base(video) { }

  override Vedio openVedio() => video;
}
public class AndroidPlatform : Platform {
  public AndroidPlatform(Vedio video) : base(video) { }

  override Vedio openVedio() => video;
}
```