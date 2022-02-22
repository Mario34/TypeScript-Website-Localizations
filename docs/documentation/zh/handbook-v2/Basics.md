---
title: 基础
layout: docs
permalink: /zh/docs/handbook/2/basic-types.html
oneline: "Step one in learning TypeScript: The basic types."
preamble: >
  <p>欢迎来到手册的第一页。如果这是你第一次使用TypeScript - 你可能想从“<a href='/zh/docs/handbook/intro.html#get-started'>入门</a>”指南之一开始
---

JavaScript中的每个值都有一组行为，您可以从运行不同的操作中观察到这些行为。这听起来很抽象，但是作为一个简单的例子，考虑我们可能在名为 `message` 的变量上运行的一些操作。

```js
// Accessing the property 'toLowerCase'
// on 'message' and then calling it
message.toLowerCase();

// Calling 'message'
message();
```

如果我们将其分解，第一行可运行的代码将访问一个名为 `toLowerCase` 的属性，然后调用它。第二个尝试直接调用 `message`。

但是，假设我们不知道 `message` 的值——这是很常见的——我们不能可靠地说，我们将从尝试运行这些代码中得到什么结果。每个操作的行为完全取决于我们一开始的值。

- `message` 是可调用的?
- 它有一个名为toLowerCase的属性吗?
- 如果它是，toLowerCase是可调用的吗?
- 如果这两个值都是可调用的，它们返回什么?

这些问题的答案通常是我们在编写JavaScript时牢记于心的，我们都希望所有的细节都是正确的。

让我们假设 `message` 是按照以下方式定义的。

```js
const message = "Hello World!";
```

正如您可能猜到的，如果我们尝试运行 `message.toLowerCase()`，我们将只得到小写的相同字符串。

那第二行代码呢?如果你熟悉 `JavaScript`，你会知道这个失败的异常:

```txt
TypeError: message is not a function
```

如果我们能避免这样的错误就太好了。

当我们运行代码时，我们的JavaScript运行时选择做什么的方式是通过计算出值的类型——它具有什么样的行为和能力。这就是 `ypeError` 所暗示的部分内容——它说字符串 `“Hello World!”` 不能作为函数调用。

对于某些值，例如基本类型 `string` 和 `number`，可以在运行时使用 `typeof` 操作符标识它们的类型。但是对于其他东西，比如函数，没有相应的运行时机制来标识它们的类型。例如，考虑以下函数:

```js
function fn(x) {
  return x.flip();
}
```

通过阅读代码，我们可以观察到，这个函数只有在给定一个具有可调用 `flip` 属性的对象时才能工作，但JavaScript不会以一种我们可以在代码运行时检查的方式显示这些信息。在纯JavaScript中，告诉 `fn` 对特定值做什么的唯一方法是调用它，然后看看发生了什么。这种行为使得在代码运行之前很难预测它将做什么，这意味着在编写代码时更难知道它将做什么。

从这种角度来看，类型是描述哪些值可以传递给 `fn`，哪些值会崩溃的概念。JavaScript只真正提供了动态类型—运行代码看看发生了什么。

另一种方法是使用静态类型系统来预测运行前的代码。

## 静态类型检查

回想一下我们之前尝试将 `string` 作为函数调用时得到的 `TypeError`。大多数人在运行他们的代码时都不喜欢出现任何类型的错误——那些错误被认为是错误!当我们写新代码时，我们会尽量避免引入新的错误。

如果我们添加一些代码，保存我们的文件，重新运行代码，并立即看到错误，我们可能能够快速隔离问题;但情况并非总是如此。我们可能没有对该特性进行足够彻底的测试，因此我们可能永远不会真正遇到可能抛出的潜在错误!或者，如果我们足够幸运地看到了错误，我们可能最终会进行大型重构，并添加许多不同的代码，我们不得不进行深入挖掘。

理想情况下，我们可以有一个工具来帮助我们在代码运行之前找到这些错误。这就是TypeScript这样的静态类型检查器所做的。静态类型系统描述了当我们运行程序时，我们的值的形状和行为。像TypeScript这样的类型检查器就会使用这些信息，并告诉我们什么时候可能会出问题。

```ts twoslash
// @errors: 2349
const message = "hello!";

message();
```

用TypeScript运行最后一个示例会在运行代码之前给我们一个错误消息。

## 非异常故障

到目前为止，我们已经讨论了一些事情，比如运行时错误——JavaScript运行时告诉我们它认为某些事情是荒谬的。之所以会出现这些情况，是因为[ECMAScript](https://tc39.github.io/ecma262/)规范有明确的说明，说明当遇到意外情况时，该语言应该如何表现。

例如，规范说，试图调用不可调用的东西应该抛出一个错误。也许这听起来像是“显而易见的行为”，但您可以想象，访问对象上不存在的属性也应该抛出错误。相反，JavaScript给了我们不同的行为，并返回值 `undefined`:

```js
const user = {
  name: "Daniel",
  age: 26,
};

user.location; // returns undefined
```

最终，静态类型系统必须调用应该在其系统中标记为错误的代码，即使它是“有效的”JavaScript，不会立即抛出错误。在TypeScript中，以下代码会产生一个关于 `location` 未定义的错误:

```ts twoslash
// @errors: 2339
const user = {
  name: "Daniel",
  age: 26,
};

user.location;
```

虽然有时这意味着要在可以表达的内容上进行权衡，但目的是捕获程序中的合法错误。TypeScript捕获了很多合法的错误。

例如:输入错误,

```ts twoslash
// @noErrors
const announcement = "Hello World!";

// How quickly can you spot the typos?
announcement.toLocaleLowercase();
announcement.toLocalLowerCase();

// We probably meant to write this...
announcement.toLocaleLowerCase();
```

未调用的函数,

```ts twoslash
// @noUnusedLocals
// @errors: 2365
function flipCoin() {
  // Meant to be Math.random()
  return Math.random < 0.5;
}
```

或者基本的逻辑错误。

```ts twoslash
// @errors: 2367
const value = Math.random() < 0.5 ? "a" : "b";
if (value !== "a") {
  // ...
} else if (value === "b") {
  // Oops, unreachable
}
```

## 类型的工具

当我们在代码中出错时，TypeScript可以捕捉到错误。这很好，但TypeScript也可以从一开始就防止我们犯这些错误。

类型检查器有信息来检查我们是否访问了变量和其他属性的正确属性。一旦它有了这些信息，它还可以开始建议您可能想要使用哪些属性。

这意味着TypeScript也可以用来编辑代码，当你在编辑器中输入时，核心的类型检查器可以提供错误消息和代码补全功能。这就是人们在谈到TypeScript中的工具时经常提到的部分内容。

<!-- prettier-ignore -->
```ts twoslash
// @noErrors
// @esModuleInterop
import express from "express";
const app = express();

app.get("/", function (req, res) {
  res.sen
//       ^|
});

app.listen(3000);
```
TypeScript非常重视工具，这不仅仅是你键入时的补全和错误。一个支持TypeScript的编辑器可以提供“快速修复”来自动修复错误，重构来轻松地重新组织代码，以及有用的导航特性来跳转到一个变量的定义，或者找到对给定变量的所有引用。所有这些都是构建在类型检查器之上的，并且是完全跨平台的，所以[你最喜欢的编辑器很可能有TypeScript支持](https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support)。

## `tsc`, TypeScript 编译器

我们已经讨论了类型检查，但是我们还没有使用我们的类型检查器。让我们来了解一下我们的新朋友 `tsc`，即TypeScript编译器。首先，我们需要通过npm获取它。

```sh
npm install -g typescript
```

> 这将全局安装TypeScript编译器 `tsc`。
> 如果你更喜欢在本地的 `node_modules` 包中运行 `tsc`，你可以使用 `npx `或类似的工具。

现在，让我们移到一个空文件夹，尝试编写我们的第一个TypeScript程序:

```ts twoslash
// Greets the world.
console.log("Hello world!");
```

注意这里没有多余的东西;这个“hello world”程序看起来和你用avaScript编写的“hello world”程序是一样的。现在让我们通过运行 `tsc` 命令来进行类型检查， `tsc` 命令是由typescript包为我们安装的。

```sh
tsc hello.ts
```

Tada!

等等，" tada "到底是什么?我们运行了 `tsc` ，什么也没发生!这里没有类型错误，所以我们没有在控制台得到任何输出，因为没有什么要报告的。

但是再检查一次——我们得到了一些文件输出。如果我们查看当前目录，我们会看到在 `hello.ts` 旁边有一个 `hello.js`  文件。这是hello的输出。tsc将其编译或转换成一个普通的JavaScript文件。如果我们检查内容，我们会看到TypeScript在处理一个 `.ts` 文件后输出了什么:

```js
// Greets the world.
console.log("Hello world!");
```

在这个例子中，TypeScript几乎没有什么要转换的，所以它看起来和我们写的东西是一样的。编译器试图发出干净可读的代码，看起来像一个人会写的东西。虽然这并不总是那么容易，但TypeScript始终会缩进，会注意我们的代码何时跨越不同的代码行，并试图保留注释。

如果我们引入了类型检查错误呢?让我们重写 `hello.ts`:

```ts twoslash
// @noErrors
// This is an industrial-grade general-purpose greeter function:
function greet(person, date) {
  console.log(`Hello ${person}, today is ${date}!`);
}

greet("Brendan");
```

If we run  `tsc hello.ts` again, notice that we get an error on the command line!

```txt
Expected 2 arguments, but got 1.
```

TypeScript告诉我们忘记给 `greet` 函数传递参数了，这是正确的。到目前为止，我们只编写了标准的JavaScript，但是类型检查仍然能够发现代码中的问题。谢谢TypeScript!

## 抛出错误

在上一个示例中，您可能没有注意到的一件事是，我们的 `hello.js` 文件再次更改了。如果我们打开那个文件，我们会看到内容基本上和我们的输入文件看起来一样。这可能有点令人惊讶，因为 `tsc` 报告了一个关于我们代码的错误，但这是基于TypeScript的核心价值之一:大多数时候，你比TypeScript更了解。

如前所述，类型检查代码限制了您可以运行的程序种类，因此需要权衡类型检查器可以接受的程序种类。大多数情况下，这是可以的，但有些情况下，这些支票会阻碍。例如，假设你将JavaScript代码迁移到TypeScript，并引入了类型检查错误。最终，您将为类型检查器进行清理，但原始的JavaScript代码已经可以工作了!为什么把它转换成TypeScript会阻止你运行它呢?

所以TypeScript不会妨碍你。当然，随着时间的推移，你可能会希望对错误有更多的防御，并让TypeScript的行为更严格一些。在这种情况下，您可以使用[`noEmitOnError`](/tsconfig#noEmitOnError)编译器选项。试着改变你的 `hello.ts` 文件和使用该标志运行 `tsc`:

```sh
tsc --noEmitOnError hello.ts
```

你会注意到 `hello.js` 从来没有更新过。

## 显式类型

到目前为止，我们还没有告诉TypeScript  `person` 或 `date` 是什么。让我们编辑代码，告诉TypeScript `person` 是一个字符串， `date` 应该是一个 `Date` 对象。我们还将在 `date` 上使用 `toDateString()` 方法。

```ts twoslash
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
```

我们所做的是在 `person` 和 `date` 上添加类型注释，以描述可以调用哪些类型的值。您可以将该签名读为“greet接受类型为 `string` 的 `person` 和类型为 `date` 的 `Date`”。

这样，TypeScript就可以告诉我们在哪些情况下，`greet` 可能被错误调用了。例如……

```ts twoslash
// @errors: 2345
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}

greet("Maddison", Date());
```

嗯?TypeScript在第二个参数上报告了一个错误，但是为什么呢?

可能令人惊讶的是，在JavaScript中调用 `Date()` 会返回一个字符串。另一方面，用新的 `Date()` 构造 `Date` 实际上给我们提供了我们所期望的东西。

无论如何，我们可以快速修正这个错误:

```ts twoslash {4}
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}

greet("Maddison", new Date());
```

请记住，我们并不总是必须编写显式类型注释。在很多情况下，TypeScript甚至可以为我们推断(或“找出”)这些类型，即使我们忽略了它们。

```ts twoslash
let msg = "hello there!";
//  ^?
```

即使我们没有告诉TypeScript `msg` 有类型 `string`，它也能查出来。这是一个特性，当类型系统最终推断出相同的类型时，最好不要添加注释。

> 注意:上面代码示例中的消息气泡。这就是你的编辑会显示的，如果你在这个词上徘徊。

## 擦除类型

让我们看看当我们用 `tsc` 编译上面的函数 `greet` 并输出JavaScript时会发生什么:

```ts twoslash
// @showEmit
// @target: es5
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}

greet("Maddison", new Date());
```

这里要注意两点:

1. `person` 和 `date` 参数不再具有类型注释。
2. 我们的“模板字符串”——使用反勾号的字符串——被转换为带有连接符(+)的普通字符串。

稍后再详细讨论第二点，现在让我们集中讨论第一点。类型注解不是JavaScript的一部分(或者说是ECMAScript的一部分)，所以实际上没有任何浏览器或其他运行时可以在没有修改的情况下运行TypeScript。这就是为什么TypeScript首先需要一个编译器——它需要一些方法来剥离或转换任何特定于TypeScript的代码，这样你才能运行它。大多数特定于typescript的代码都被删除了，同样地，这里我们的类型注释也被完全删除了。

> **记住**: 类型注释永远不会改变程序的运行时行为。.

## 底层

与上面的另一个区别是我们的模板字符串被重写了

```js
`Hello ${person}, today is ${date.toDateString()}!`;
```

to

```js
"Hello " + person + ", today is " + date.toDateString() + "!";
```

为什么会这样呢?

模板字符串是ECMAScript 2015版本(也就是ECMAScript 6、ES2015、ES6等——别问了)的一个特性。TypeScript能够将新版本的ECMAScript代码重写为旧版本的ECMAScript 3或ECMAScript 5(又名ES3和ES5)。从ECMAScript的新版本或“更高”版本降级到旧版本或“更低”版本的过程有时称为降级。

默认情况下，TypeScript的目标是ES3，这是一个非常旧的ECMAScript版本。我们可以通过使用 [`target`](/tsconfig#target) 选项来选择一些更近期的东西。使用 `--target es2015` 运行会将TypeScript更改为目标ECMAScript 2015，这意味着代码应该能够在任何支持ECMAScript 2015的地方运行。运行 `tsc -target es2015`，你好。t给出如下输出:

```js
function greet(person, date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
greet("Maddison", new Date());
```

> 虽然默认目标是ES3，但目前绝大多数浏览器都支持ES2015。
> 因此，大多数开发者可以安全地指定ES2015或以上版本作为目标，除非与某些古老的浏览器兼容是重要的。

## 严格模式

不同的用户来到TypeScript，在类型检查器中寻找不同的东西。有些人正在寻找一种更宽松的选择加入体验，这种体验可以帮助验证他们的程序的某些部分，并且仍然有体面的工具。这是TypeScript的默认体验，在这里类型是可选的，推理采用最宽松的类型，并且不需要检查潜在的`null`/`undefined`值。就像 `tsc` 在遇到错误时发出的那样，这些默认设置是为了不妨碍您。如果您正在迁移现有的JavaScript，这可能是理想的第一步。

相比之下，很多用户更喜欢让TypeScript尽可能直接地验证，这就是为什么该语言还提供了严格性设置。这些严格性设置将静态类型检查从开关(无论您的代码是否被检查)变成接近拨号的东西。你把这个拨号调得越高，TypeScript会为你检查得越多。这可能需要一些额外的工作，但总的来说，从长远来看，这是值得的，并且支持更彻底的检查和更准确的工具。如果可能，新代码库应该总是打开这些严格检查。

TypeScript有几个类型检查的严格标志，可以打开或关闭，我们所有的例子都是启用的，除非另有说明。CLI中的 [`strict`](/tsconfig#strict) 标志，或者[`tsconfig.json`](/zh/docs/handbook/tsconfig-json.html)中的 `"strict": true` 会同时切换它们，但我们可以单独选择不启用它们。你应该知道的两个最大的是[`noImplicitAny`]和[`strictNullChecks`](/tsconfig#strictNullChecks)。

## `noImplicitAny`

回想一下，在某些地方，TypeScript并不尝试为我们推断类型，而是退回到最宽松的类型: `any`。这并不是最糟糕的事情——毕竟，回归到任何东西都只是简单的JavaScript体验。

然而，使用 `any` 通常首先就违背了使用TypeScript的目的。程序的类型越多，得到的验证和工具就越多，这意味着在编写代码时遇到的bug就越少。打开[`noImplicitAny`](/tsconfig#noImplicitAny)标志将对隐式推断为 `any` 类型的任何变量发出错误。

## `strictNullChecks`

默认情况下，像 `null` 和 `undefined` 这样的值可以分配给任何其他类型。这可以使编写一些代码更容易，但忘记处理 `null` 和 `undefined` 是导致世界上无数错误的原因——有些人认为这是一个[价值10亿美元的错误](https://www.youtube.com/watch?v=ybrQvs4x0Ps)![`strictNullChecks`](/tsconfig#strictNullChecks)标志使对 `null` 和 `undefined` 的处理更加明确，并使我们不必担心是否忘记处理 `null` 和 `undefined` 。
