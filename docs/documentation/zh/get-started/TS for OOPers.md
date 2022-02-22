---
title: 
short: 对于Java/C#发者的Typescript
layout: docs
permalink: /zh/docs/handbook/typescript-in-5-minutes-oop.html
oneline: Learn TypeScript if you have a background in object-oriented languages
---

TypeScript 是习惯于使用其他静态类型语言（例如 C# 和 Java）的程序员的合适选择。

TypeScript 的类型系统提供了许多相同的好处，例如更好的代码完成、更早的错误检测以及程序各部分之间更清晰的通信。虽然 TypeScript 为这些开发人员提供了许多熟悉的功能，但值得回顾一下 JavaScript（以及 TypeScript）与传统 OOP 语言的不同之处。了解这些差异将帮助您编写更好的 JavaScript 代码，并避免程序员直接从 C#/Java 转向 TypeScript 可能陷入的常见陷阱。

## 共同学习 JavaScript

如果您已经熟悉 JavaScript，但主要是 Java 或 C# 程序员，则此介绍性页面可以帮助解释您可能容易受到的一些常见误解和陷阱。TypeScript 建模类型的一些方式与 Java 或 C# 完全不同，在学习 TypeScript 时记住这些很重要。

如果您是 Java 或 C# 程序员，一般不熟悉 JavaScript，我们建议您先学习一点不带类型的 JavaScript，以了解 JavaScript 的运行时行为。因为 TypeScript 不会改变你的代码运行方式，所以你仍然需要学习 JavaScript 的工作原理才能编写出真正能做某事的代码！

重要的是要记住，TypeScript 使用与 JavaScript 相同的运行时，因此任何关于如何完成特定运行时行为（将字符串转换为数字、显示警报、将文件写入磁盘等）的资源都将同样适用于TypeScript 程序。不要将自己限制在特定于 TypeScript 的资源中！

## 重新思考Class

C# 和 Java 是我们所说的强制性 OOP语言。在这些语言中，类是代码组织的基本单元，也是运行时所有数据和行为的基本容器。对于某些问题，强制所有功能和数据保存在类中可能是一个很好的领域模型，但并不是每个领域都需要以这种方式表示。

### 自由函数和数据

在 JavaScript 中，函数可以存在于任何地方，并且数据可以自由传递，而无需在预定义的class或struct. 这种灵活性非常强大。在没有隐含 OOP 层次结构的情况下处理数据的“免费”函数（与类无关的函数）往往是用 JavaScript 编写程序的首选模型。

### 静态类

此外，TypeScript 中不需要来自 C# 和 Java 的某些构造，例如单例和静态类。

## TypeScript 中的 OOP

也就是说，如果你愿意，你仍然可以使用类！有些问题非常适合通过传统的 OOP 层次结构来解决，TypeScript 对 JavaScript 类的支持将使这些模型更加强大。TypeScript 支持许多常见模式，例如实现接口、继承和静态方法。

我们将在本指南后面介绍类。

## 重新思考类型

TypeScript 对类型的理解实际上与 C# 或 Java 完全不同。让我们探讨一些差异。

### 名义具体化类型系统

在 C# 或 Java 中，任何给定的值或对象都具有一种确切的类型——null原始类型或已知的类类型。我们可以在运行时调用类似 `value.GetType()` 或的方法 `value.getClass()` 来查询确切的类型。这种类型的定义将驻留在某个具有某个名称的类中，除非有明确的继承关系或共同实现的接口，否则我们不能使用具有相似形状的两个类来代替彼此。

这些方面描述了一个具体化的、名义上的类型系统。我们在代码中编写的类型在运行时存在，并且这些类型通过它们的声明而不是它们的结构相关联。

### 作为集合的类型

在 C# 或 Java 中，考虑运行时类型与其编译时声明之间的一一对应关系是有意义的。

在 TypeScript 中，最好将类型视为一组具有共同点的值。因为类型只是集合，一个特定的值可以同时属于许多集合。

一旦您开始将类型视为集合，某些操作就会变得非常自然。例如，在c#中，传递一个字符串或int型的值是很尴尬的，因为没有一个单独的类型来表示这种类型的值。

在 TypeScript 中，一旦您意识到每种类型都只是一个集合，这将变得非常自然。您如何描述属于string集合或number集合的值？它只是属于这些集合的并集：`string | number`.

TypeScript 提供了许多以集合论方式处理类型的机制，如果您将类型视为集合，您会发现它们更直观。

### 擦除结构类型

在 TypeScript 中，对象不是单一的确切类型。例如，如果我们构造一个满足接口的对象，即使两者之间没有声明性关系，我们也可以在期望该接口的地方使用该对象。

```ts twoslash
interface Pointlike {
  x: number;
  y: number;
}
interface Named {
  name: string;
}

function logPoint(point: Pointlike) {
  console.log("x = " + point.x + ", y = " + point.y);
}

function logName(x: Named) {
  console.log("Hello, " + x.name);
}

const obj = {
  x: 0,
  y: 0,
  name: "Origin",
};

logPoint(obj);
logName(obj);
```

TypeScript的类型系统是结构化的，而不是名义上的:我们可以将 `obj` 用作 `Pointlike` 类型，因为它的 `x` 和 `y` 属性都是数字。类型之间的关系取决于它们所包含的属性，而不是它们是否用某种特定的关系声明。

TypeScript的类型系统也没有具体化:运行时没有任何东西告诉我们 `obj` 是 `Pointlike` 的。事实上，类点类型在运行时并不以任何形式出现。

回到类型作为集合的思想，我们可以把 `obj` 看作是类点值集和命名值集的成员。

### 结构类型的后果

OOP 程序员经常对结构类型的两个特定方面感到惊讶。

#### 空类型

首先是空类型似乎出乎意料：

```ts twoslash
class  ` {}

function fn(arg:  `) {
  // do something?
}

// No error, but this isn't an 'Empty' ?
fn({ k: 10 });
```


TypeScript通过查看提供的参数是否为有效的 `Empty` 来判断这里对fn的调用是否有效。它通过检查 `{k: 10}` 和 `Empty{}` 类的结构来做到这一点。我们可以看到 `{k: 10}` 具有 `Empty` 所具有的所有属性，因为Empty没有属性。因此，这是一个有效的调用!

这可能看起来令人惊讶，但它最终与名义OOP语言中实施的关系非常相似。子类不能删除基类的属性，因为这样做会破坏派生类与其基类之间的自然子类型关系。结构类型系统简单地通过描述具有兼容类型属性的子类型来隐式地标识这种关系。

#### 相同类型

另一个常见的惊喜来源是相同的类型：

```ts
class Car {
  drive() {
    // hit the gas
  }
}
class Golfer {
  drive() {
    // hit the ball far
  }
}

// No error?
let w: Car = new Golfer();
```

同样，这不是错误，因为这些类的结构是相同的。虽然这似乎是一个潜在的混淆来源，但在实践中，不应该相关的相同类并不常见。

我们将在“类”一章中了解更多关于类如何相互关联的信息。

### 反射

OOP programmers are accustomed to being able to query the type of any value, even a generic one:

```csharp
// C#
static void LogType<T>() {
    Console.WriteLine(typeof(T).Name);
}
```

因为 TypeScript 的类型系统被完全擦除，所以关于泛型类型参数的实例化等信息在运行时不可用。

JavaScript确实有一些有限的类型语句，如 `typeof` 和 `instanceof` ，但请记住，这些操作符仍然在处理存在于类型删除输出代码中的值。例如，`typeof (new Car())` 将是 `“object”`，而不是 `Car` 或 `“Car”` 。

## 下一步

这是对日常 TypeScript 中使用的语法和工具的简要概述。从这里，您可以：

- [从头到尾](/zh/docs/handbook/intro.html) 阅读完整的手册(30m)(30m)
- 探索 [Playground 示例](/play#show-examples)

