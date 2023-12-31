---
title: Composite（组合）模式
tags: 
  - 设计模式
date: 2023-09-19
---

{% plantuml %}
!theme sketchy-outline

class Client

abstract class Component
{
#name:String
  + Component(String) : void
  + doSomething() : void
}

class Leaf
{
  + Component(String) : void
  + doSomething() : void
}

class Composite
{
-components:List<Component>
  + Component(String) : void
  + doSomething() : void
  + addChild(Component) : void
  + getChildren(int) : Component
  + removeChild(Component) : void
}

Component <-left- Client
Component <|-- Leaf
Component <|-- Composite
Component <--o Composite

{% endplantuml %}

### 意图
将对象合成树形结构以表示“部分-整体”的层次结构。Composite 使得用户对单个对象和组合对象的使用具有一致性。

### 适用性
* 想表示对象的部分-整体层次结构；
* 希望用户忽略组合对象与单个对象的不同,用户将统一地使用组合结构中的所有对象。

### 代码示例
```c#
static IEnumerable<string> NameYield(AbstractFile file) {
  yiled file.Name;
  List<AbstractFile> childrens= file.getChildren();
  if(childrens != null) {
    foreach(AbstractFile child in childrens) {
      yield return child.Name;
      NameYield(child);
    }
  }
}

/// 为组合中的对象声明接口;在适当情况下实现所有类共有接口的默认行为;
abstract class AbstractFile {
  protected string name;

  public string Name { get { return this.name; }}

  public abstract bool Add(AbstractFile file);
  public abstract bool Remove(AbstractFile file);
  public abstract List<AbstractFile> getChildren();
}

class Folder : AbstractFile {
  // 定义有子组件的那些组件的行为:存储子组件;在 Component 接口中实现与子组件有关的操作。
  private List<AbstractFile> childrens = new List<AbstractFile>();

  public Folder(string name) {
    this.name = name;
  }

  override bool Add(AbstractFile file) => this.childrens.Add(file);
  override bool Remove(AbstractFile file) => this.childrens.Remove(file);
  override List<AbstractFile> getChildren() => this.childrens;
}

/// 在组合中表示叶结点对象，叶结点没有子结点;在组合中定义图元对象的行为。
class File : AbstractFile {
  public File(string name) {
    this.name = name;
  }

  // 保持一致性
  override bool Add(AbstractFile file) => false;
  override bool Remove(AbstractFile file) => false;
  override List<AbstractFile> getChildren() => null;
}
```