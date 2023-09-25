---
title: Flyweight （享元）模式
tags: 
  - 设计模式
date: 2023-09-24
---

{% plantuml %}
!theme sketchy-outline

class FlyweightFactory
{
	- mMap : HashMap
	+ getFlyweight()
}
note right: 享元工厂

class Flyweight
{
	+ dosomething()
}
note right: 享元对象抽象基类或者接口

class ConcreteFlyweight
{
	- intrinsicState : String
	+ dosomething()
}
note right: 具体的享元对象

FlyweightFactory *-- Flyweight
Flyweight <|-- ConcreteFlyweight

{% endplantuml %}

### 意图
运用共享技术有效的支持大量细粒度的对象。

### 适用性
* 一个应用程序使用了大量的对象；
* 完全由于使用大量的对象，造成很大的存储开销；
* 对象的大多数状态都可变为外部状态；
* 如果删除对象的外部状态，那么可以用相对较少的共享对象取代很多组对象；
* 应用程序不依赖于对象标识。由于 *Flyweight* 对象可以被共享，所以对于概念上明显有别的对象，标识测试将返回真值。

### 优缺点
* 极大减少内存中相似或相同对象数量，节约系统资源，提供系统性能；
* 提高效率，由于创建对象的数量减少，所以对系统内存的需求也减小，使得速度更快，效率更高；
* 享元模式中的外部状态相对独立，且不影响内部状态。

---

* 为了使对象可以共享，需要将享元对象的部分状态外部化，分离内部状态和外部状态，使程序逻辑复杂。

### 代码示例
```c#
enum Color {
	white,
	black
}
abstract class Piece {
	protected Color color;

	public abstract void draw(int x, int y);
}
class WhitePiece : Piece {
	public WhitePiece() {
		this.color = Color.white
	}

	public override void draw(int x, int y) => $"drawpiece with color: {this.color}; point at ({x},{y})";
}
class BlackPiece : Piece {
	public BlackPiece() {
		this.color = Color.black
	}

	public override void draw(int x, int y) => $"drawpiece with color: {this.color}; point at ({x},{y})";
}

class PieceFactory {
	private Piece[] pieces = new {new WhitePiece(), new BlockPiece()};

	public Piece getPiece(int key) {
		if(key == 0) return this.pieces[0];
		else return this.pieces[1];
	}
}
```
