---
title: 对于JavaScript开发者的Typescript
short: TypeScript for JS Programmers
layout: docs
permalink: /zh/docs/handbook/typescript-in-5-minutes.html
oneline: Learn how TypeScript extends JavaScript
---

TypeScript 与 JavaScript 之间有着不同寻常的关系。TypeScript 提供了所有的 JavaScript 特性，并且在这些特性之上还有一个额外的层:TypeScript 的类型系统。

例如，JavaScript 提供了类似 `string` 和 `number` 的原始类型，但它不会检查您是正确的使用这些类型,TypeScript 可以。

这意味着您现有的工作 JavaScript 代码也是 TypeScript 代码。TypeScript 的主要好处是它可以突出代码中的意外行为，从而降低出现错误的机会。

本教程提供了 TypeScript 的简要概述，重点介绍了它的类型系统。

## 类型推导

TypeScript 支持 JavaScript 推导，并且会在许多情况下为您生成类型。
例如，在创建变量并将其分配给特定值时，TypeScript 将使用该值作为其类型。

```ts twoslash
let helloWorld = "Hello World";
//  ^?
```

通过了解 JavaScript 的工作原理，TypeScript 可以构建一个接受 JavaScript 代码但具有类型的类型系统。这提供了一个类型系统，而无需添加额外的字符来使代码中的类型显式。 
这就是 TypeScript 如何推导出示例中的 `helloWorld` 为 `string` 类型的方式。

您可能已经在 Visual Studio Code 中编写了 JavaScript，并且拥有编辑器自动完成功能。
Visual Studio Code 在后台使用 TypeScript 以更轻松地使用 JavaScript。

## 定义类型

您可以在 JavaScript 中使用多种设计模式。但是，某些设计模式难以自动推断类型（例如，使用动态编程的模式）。为了涵盖这些情况，TypeScript 支持 JavaScript 语言的扩展，它为您提供了告诉 TypeScript 类型应该是什么的地方。

例如，要创建一个包含 `name: string` 和 `id: number` 类型的对象, 您可以编写：

```ts twoslash
const user = {
  name: "Hayes",
  id: 0,
};
```

您可以使用声明显式描述此对象的细节 `interface`：

```ts twoslash
interface User {
  name: string;
  id: number;
}
```

然后，您可以使用类似于在变量声明之后的 `interface` 语法来声明 JavaScript 对象符合您的新对象的形状：`: TypeName`

```ts twoslash
interface User {
  name: string;
  id: number;
}
// ---cut---
const user: User = {
  name: "Hayes",
  id: 0,
};
```

如果你提供的对象与你提供的接口不匹配，TypeScript 会警告你：

```ts twoslash
// @errors: 2322
interface User {
  name: string;
  id: number;
}

const user: User = {
  username: "Hayes",
  id: 0,
};
```

由于 JavaScript 支持类和面向对象的编程，TypeScript 也是如此。您可以将接口声明与类一起使用：

```ts twoslash
interface User {
  name: string;
  id: number;
}

class UserAccount {
  name: string;
  id: number;

  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}

const user: User = new UserAccount("Murphy", 1);
```

您可以使用接口来注释参数并将值返回给函数：

```ts twoslash
// @noErrors
interface User {
  name: string;
  id: number;
}
// ---cut---
function getAdminUser(): User {
  //...
}

function deleteUser(user: User) {
  // ...
}
```

JavaScript 中已经有一小部分可用的原始类型： `boolean`, `bigint`, `null`, `number`, `string`, `symbol`, and `undefined`, 您可以在接口中使用它们。TypeScript 对该列表进行了更多扩展，例如 `any` (允许任何内容), [`unknown`](/play#example/unknown-and-never) (确保使用此类型的人声明该类型是什么), [`never`](/play#example/unknown-and-never) (这种类型不可能发生), and `void` (返回 `undefined` 或不返回的函数价值).

您将看到构建类型有两种语法： [接口和类型](/play/?e=83#example/types-vs-interfaces), 你应该更偏向使用 `interface`, 而 `type` 仅在特定功能时使用.

## 组合类型

在TypeScript中，你可以通过组合简单的类型来创建复杂的类型。有两种有效的方法可以做到这一点:使用联合和使用泛型。

### 联合类型

使用联合，可以声明一个新类型可以是多种类型中的一种。例如，你可以将一个布尔类型描述为 `true` 或 `false :

```ts twoslash
type MyBool = true | false;
```

注意：如果您将鼠标悬停在 `MyBool` 上方，您会看到它被归类为 `boolean`. 这是结构类型系统的一个属性。更多关于这下面。
联合类型的一个流行用例是描述允许值是的一组 `string` 或 `number` [字面量](/zh/docs/handbook/2/everyday-types.html#literal-types)：

```ts twoslash
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

联合也提供了一种处理不同类型的方法。例如，您可能会提供一个 `array` 或者 `string` 的类型:

```ts twoslash
function getLength(obj: string | string[]) {
  return obj.length;
}
```

要了解变量的类型，请使用 `typeof`:

| Type      | Predicate                          |
| --------- | ---------------------------------- |
| string    | `typeof s === "string"`            |
| number    | `typeof n === "number"`            |
| boolean   | `typeof b === "boolean"`           |
| undefined | `typeof undefined === "undefined"` |
| function  | `typeof f === "function"`          |
| array     | `Array.isArray(a)`                 |

例如，您可以使函数返回不同的值，具体取决于传递的是字符串还是数组：

<!-- prettier-ignore -->
```ts twoslash
function wrapInArray(obj: string | string[]) {
  if (typeof obj === "string") {
    return [obj];
//          ^?
  }
  return obj;
}
```

### 泛型

泛型为类型提供变量。一个常见的例子是数组。没有泛型的数组可以包含任何东西。具有泛型的数组可以描述数组包含的值。

```ts
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

您可以声明自己的使用泛型的类型：

```ts twoslash
// @errors: 2345
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;

// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();

// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
```

## 结构类型系统

TypeScript 的核心原则之一是类型检查侧重于值的形状。这有时被称为“鸭子打字”或“结构打字”。

在结构类型系统中，如果两个对象具有相同的形状，则认为它们属于同一类型。

```ts twoslash
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```

变量永远不会被 `point` 声明为 `Point` 类型。但是，TypeScript在类型检查point中将形状与形状进行比较。`Point` 它们具有相同的形状，因此代码通过。

形状匹配只需要匹配对象字段的子集。

```ts twoslash
// @errors: 2345
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
// ---cut---
const point3 = { x: 12, y: 26, z: 89 };
logPoint(point3); // logs "12, 26"

const rect = { x: 33, y: 3, width: 30, height: 80 };
logPoint(rect); // logs "33, 3"

const color = { hex: "#187ABF" };
logPoint(color);
```

类和对象如何符合形状之间没有区别：

```ts twoslash
// @errors: 2345
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
// ---cut---
class VirtualPoint {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56"
```

如果对象或类具有所有必需的属性，TypeScript 会说它们匹配，而不管实现细节如何。

## 下一步

这是对日常 TypeScript 中使用的语法和工具的简要概述。从这里，您可以：

- [从头到尾](/zh/docs/handbook/intro.html) 阅读完整的手册(30m)(30m)
- 探索 [Playground 示例](/play#show-examples)
