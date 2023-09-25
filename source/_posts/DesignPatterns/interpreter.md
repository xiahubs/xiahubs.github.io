---
title: Interpreter（解释器）模式
tags: 
  - 设计模式
date: 2023-09-25
---

{% plantuml %}
!theme sketchy-outline

abstract class AbstractExpression
{
  + interpret(context)
}
note top of AbstractExpression: 抽象表达式

class TerminalExpression
{
  + interpret(context)
}
note bottom of TerminalExpression: 终结符表达式

class NonTerminalExpression
{
  + interpret(context)
}
note bottom of NonTerminalExpression: 非终结符表达式

class Context
note top of Context: 上下文

AbstractExpression <--o NonTerminalExpression
AbstractExpression<|-- NonTerminalExpression
AbstractExpression <|-- TerminalExpression
AbstractExpression <-- Client
Context <-- Client

{% endplantuml %}

### 意图
给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。

### 适用性
*Interpreter* 模式适用于当有一个语言需要解释执行，并且可将该语言中的句子表示为一个抽象语法树时，以下情况效果最好:
* 该文法简单。对于复杂的发文，文法的类层次变得庞大而无法管理。此时语法分析程序生成器这样的工具是更好的选择。它们无须构建抽象语法树即可解释表达式，这样可以节省空间还可能节省时间；
* 效率不是一个关键问题。最高效的解释器通常不是通过直接解释语法分析树实现的，而是首先将它们转换成另一种形式。不过，即使在这种情况下，转换器仍然可用该模式实现。