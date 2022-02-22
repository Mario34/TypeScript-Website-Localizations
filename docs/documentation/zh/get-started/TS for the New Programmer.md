---
title: TypeScript新手指南
short: TS新手指南
layout: docs
permalink: /zh/docs/handbook/typescript-from-scratch.html
oneline: Learn TypeScript from scratch
---

恭喜您选择 TypeScript 作为您的首选语言之一——您已经做出了正确的决定！

您可能已经听说过 TypeScript 是 JavaScript 的“变种”或“变体”。TypeScript (TS) 和 JavaScript (JS) 之间的关系在现代编程语言中相当独特，因此了解更多关于这种关系的信息将有助于您了解 TypeScript 如何添加到 JavaScript 中。

## 什么是 JavaScript？一个简短的历史

JavaScript（也称为 ECMAScript）作为一种简单的浏览器脚本语言开始了它的生命。在它被发明的时候，它被期望用于嵌入网页中的短代码片段——编写超过几十行代码有点不寻常。因此，早期的 Web 浏览器执行此类代码的速度非常慢。然而，随着时间的推移，JS 变得越来越流行，Web 开发人员开始使用它来创建交互式体验。

Web 浏览器开发人员通过优化他们的执行引擎（动态编译）和扩展可以用它做的事情（添加 API）来应对这种增加的 JS 使用，这反过来又使 Web 开发人员更多地使用它。在现代网站上，您的浏览器经常运行跨越数十万行代码的应用程序。这是“网络”的长期而渐进的发展，从一个简单的静态页面网络开始，逐渐演变为各种丰富应用程序的平台。

不仅如此，JS 已经变得足够流行，可以在浏览器之外使用，例如使用 node.js 实现 JS 服务器。JS 的“随处运行”特性使其成为跨平台开发的有吸引力的选择。现在有许多开发人员只使用JavaScript 来编写他们的整个堆栈！

总而言之，我们有一种为快速使用而设计的语言，然后发展成为一种成熟的工具，可以编写数百万行的应用程序。每种语言都有自己的怪癖——古怪和惊喜，而 JavaScript 的不起眼的开端使它拥有许多这样的怪癖。一些例子：

- JavaScript 的相等运算符 ( ==)强制其参数，导致意外行为：

  ```js
  if ("" == 0) {
    // It is! But why??
  }
  if (1 < x < 3) {
    // True for *any* value of x!
  }
  ```

- JavaScript 还允许访问不存在的属性：

  ```js
  const obj = { width: 10, height: 15 };
  // Why is this NaN? Spelling is hard!
  const area = obj.width * obj.heigth;
  ```

大多数编程语言会在发生此类错误时抛出错误，有些会在编译期间（在任何代码运行之前）这样做。在编写小程序时，这种怪癖很烦人但可以控制；在编写具有数百或数千行代码的应用程序时，这些不断出现的意外是一个严重的问题。

## TypeScript: A Static Type Checker

我们之前说过，有些语言根本不允许那些有缺陷的程序运行。在不运行代码的情况下检测代码中的错误称为静态检查。根据所操作的值的类型来确定什么是错误，什么不是，这被称为静态类型检查。

TypeScript 在执行之前检查程序是否存在错误，并根据值的种类进行检查，它是一个静态类型检查器。例如，上面的最后一个示例由于obj. 这是 TypeScript 发现的错误：

```ts twoslash
// @errors: 2551
const obj = { width: 10, height: 15 };
const area = obj.width * obj.heigth;
```

### JavaScript 的类型化超集

TypeScript 与 JavaScript 有什么关系呢？

#### Syntax

TypeScript 是一种语言，它是 JavaScript 的超集：因此 JS 语法是合法的 TS。语法是指我们编写文本以形成程序的方式。例如，此代码有一个语法错误，因为它缺少一个)：

```ts twoslash
// @errors: 1005
let a = (4
```

由于其语法，TypeScript 不会将任何 JavaScript 代码视为错误。这意味着您可以获取任何有效的 JavaScript 代码并将其放入 TypeScript 文件中，而不必担心它是如何编写的。

#### Types

然而，TypeScript 是一个类型化的超集，这意味着它添加了关于如何使用不同类型的值的规则。上面 `obj.heigth` 的错误不是语法错误：它是以不正确的方式使用某种值（类型）的错误。

再举一个例子，这是可以在浏览器中运行的 JavaScript 代码，它会记录一个值：

```js
console.log(4 / []);
```

这个程序会输出 `Infinity`。
但是，TypeScript 认为将数字除以数组是一种无意义的操作，并且会发出错误：

```ts twoslash
// @errors: 2363
console.log(4 / []);
```

您可能确实打算将一个数字除以一个数组，也许只是为了看看会发生什么，但大多数时候，这是一个程序错误。TypeScript 的类型检查器只允许允许正确的程序通过，同时仍能捕获尽可能多的常见错误。（稍后，我们将了解 TypeScript 检查代码的严格程度的设置。）

如果您将一些代码从 JavaScript 文件移动到 TypeScript 文件，您可能会看到类型错误，具体取决于代码的编写方式。这些可能是代码的合理问题，或者 TypeScript 过于保守。在本指南中，我们将演示如何添加各种 TypeScript 语法来消除此类错误。

#### 运行时行为

TypeScript 也是一种保留JavaScript运行时行为的编程语言。
例如，在 JavaScript 中除以零会产生 `Infinity` 而不是抛出运行时异常。
作为一项原则，TypeScript永远不会改变 JavaScript 代码的运行时行为。

这意味着如果您将代码从 JavaScript 移动到 TypeScript，即使 TypeScript 认为代码存在类型错误，也可以保证以相同的方式运行。

保持与 JavaScript 相同的运行时行为是 TypeScript 的基本承诺，因为这意味着您可以轻松地在两种语言之间转换，而不必担心可能导致程序停止工作的细微差异。

<!--
Missing subsection on the fact that TS extends JS to add syntax for type
specification.  (Since the immediately preceding text was raving about
how JS code can be used in TS.)
-->

#### 擦除类型

粗略地说，一旦 TypeScript 的编译器检查完你的代码，它就会删除类型以生成生成的“编译”代码。
这意味着一旦你的代码被编译，生成的纯 JS 代码就没有类型信息。

这也意味着 TypeScript 永远不会根据它推断的类型改变程序的行为。
底线是，虽然您可能会在编译期间看到类型错误，但类型系统本身与程序运行时的工作方式无关。

最后，TypeScript 不提供任何额外的运行时库。
您的程序将使用与 JavaScript 程序相同的标准库（或外部库），因此无需学习其他特定于 TypeScript 的框架。

## 学习 JavaScript 和 TypeScript

我们经常看到“我应该学习 JavaScript 还是 TypeScript？”这个问题。

答案是不学习 JavaScript 就无法学习 TypeScript！TypeScript 与 JavaScript 共享语法和运行时行为，因此您学习 JavaScript 的任何内容都可以同时帮助您学习 TypeScript。

有很多很多资源可供程序员学习 JavaScript；如果您正在编写 TypeScript，则不应忽略这些资源。例如，标记的 StackOverflow 问题大约是 20 倍，javascript但typescript所有问题javascript也适用于 TypeScript。

如果您发现自己正在搜索“如何在 TypeScript 中对列表进行排序”之类的内容，请记住：TypeScript 是带有编译时类型检查器的 JavaScript 运行时。在 TypeScript 中对列表进行排序的方式与在 JavaScript 中的排序方式相同。如果您找到直接使用 TypeScript 的资源，那也很好，但不要局限于认为您需要针对有关如何完成运行时任务的日常问题的特定于 TypeScript 的答案。

## Next Steps

This was a brief overview of the syntax and tools used in everyday TypeScript. From here, you can:

- Learn some of the JavaScript fundamentals, we recommend either:

  - [Microsoft's JavaScript Resources](https://docs.microsoft.com/javascript/) or
  - [JavaScript guide at the Mozilla Web Docs](https://developer.mozilla.org/docs/Web/JavaScript/Guide)

- Continue to [TypeScript for JavaScript Programmers](/zh/docs/handbook/typescript-in-5-minutes.html)
- Read the full Handbook [from start to finish](/zh/docs/handbook/intro.html) (30m)
- Explore the [Playground examples](/play#show-examples)

<!-- Note: I'll be happy to write the following... -->
<!--
## Types

    * What's a type? (For newbies)
      * A type is a *kind* of value
      * Types implicitly define what operations make sense on them
      * Lots of different kinds, not just primitives
      * We can make descriptions for all kinds of values
      * The `any` type -- a quick desctiption, what it is, and why it's bad
    * Inference 101
      * Examples
      * TypeScript can figure out types most of the time
      * Two places we'll ask you what the type is: Function boundaries, and later-initialized values
    * Co-learning JavaScript
      * You can+should read existing JS resources
      * Just paste it in and see what happens
      * Consider turning off 'strict' -->
