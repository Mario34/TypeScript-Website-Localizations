---
title: TypeScript 手册
layout: docs
permalink: /zh/docs/handbook/intro.html
oneline: Your first step to learn TypeScript
handbook: "true"
---

## 关于本手册

在被引入编程社区20多年后，JavaScript现在是有史以来最广泛的跨平台语言之一。JavaScript最初是一种为网页添加简单交互性的小型脚本语言，后来发展成为各种大小的前端和后端应用程序的首选语言。虽然用JavaScript编写的程序的规模、范围和复杂性呈指数级增长，但JavaScript语言表达不同代码单元之间关系的能力却没有增长。再加上JavaScript相当奇特的运行时语义，这种语言和程序复杂性之间的不匹配使得JavaScript开发成为一项难以大规模管理的任务。

程序员编写的最常见的错误类型可以描述为类型错误:在期望不同类型的值时使用了某种类型的值。这可能是由于简单的输入错误、未能理解库的API表面、对运行时行为的不正确假设或其他错误。TypeScript的目标是成为JavaScript程序的静态类型检查器——换句话说，它是一个在你的代码运行之前运行的工具(静态)，并确保程序的类型是正确的(类型检查)。

如果你在没有JavaScript背景的情况下开始使用TypeScript，并希望TypeScript成为你的第一语言，我们建议你首先阅读[Microsoft Learn JavaScript tutorial](https://docs.microsoft.com/javascript/)教程中的文档，或者阅读[JavaScript at the Mozilla Web Docs](https://developer.mozilla.org/docs/Web/JavaScript/Guide)。如果您有其他语言的经验，您应该能够通过阅读手册快速掌握JavaScript语法。

## 这本手册的结构是怎样的

手册分为两部分：

- **手册**

  TypeScript 手册旨在成为向日常程序员解释 TypeScript 的综合文档。您可以在左侧导航中从上到下阅读手册。
  您应该期望每一章或每一页都能为您提供对给定概念的深刻理解。TypeScript 手册不是一个完整的语言规范，但它旨在成为该语言所有特性和行为的综合指南。
  完成演练的读者应该能够：

  - 阅读并理解常用的 TypeScript 语法和模式
  - 解释重要编译器选项的影响
  - 在大多数情况下正确预测类型系统行为

  为清晰和简洁起见，本手册的主要内容不会探讨所涵盖功能的所有边缘情况或细节。您可以在参考文章中找到有关特定概念的更多详细信息。

- **参考文件**

  导航中手册下方的参考部分旨在提供对 TypeScript 特定部分如何工作的更丰富的理解。您可以从上到下阅读它，但每个部分都旨在为单个概念提供更深入的解释 - 这意味着没有连续性的目标。

### 非目标

该手册还旨在成为一份简洁的文档，可以在几个小时内轻松阅读。为了简短起见，某些主题不会被涵盖。

具体来说，该手册没有完全介绍核心 JavaScript 基础知识，如函数、类和闭包。在适当的情况下，我们将包含指向背景阅读的链接，您可以使用这些链接来阅读这些概念。

该手册也不打算替代语言规范。在某些情况下，将跳过边缘情况或对行为的正式描述，转而采用更易于理解的高级解释。相反，有单独的参考页面更准确、更正式地描述了 TypeScript 行为的许多方面。参考页面不适用于不熟悉 TypeScript 的读者，因此他们可能会使用您尚未阅读的高级术语或参考主题。

最后，除非必要，否则本手册不会涵盖 TypeScript 如何与其他工具交互。诸如如何使用 webpack、rollup、parcel、react、babel、closure、lerna、rush、bazel、preact、vue、angular、svelte、jquery、yarn 或 npm 配置 TypeScript 等主题超出了范围 - 您可以在其他地方找到这些资源在网上。

## 开始使用

在开始使用[The Basics](/zh/docs/handbook/2/basic-types.html)之前，我们建议您阅读以下介绍性页面之一。这些介绍旨在突出 TypeScript 与您喜欢的编程语言之间的主要相似之处和不同之处，并澄清针对这些语言的常见误解。

- [新程序员的 TypeScript](/zh/docs/handbook/typescript-from-scratch.html)
- [适用于 JavaScript 程序员的 TypeScript](/zh/docs/handbook/typescript-in-5-minutes.html)
- [面向 OOP 程序员的 TypeScript](/zh/docs/handbook/typescript-in-5-minutes-oop.html)
- [面向函数式程序员的 TypeScript](/zh/docs/handbook/typescript-in-5-minutes-func.html)

否则，请跳至基础知识或获取[The Basics](/zh/docs/handbook/2/basic-types.html)或[Epub](/assets/typescript-handbook.epub) or [PDF](/assets/typescript-handbook.pdf)格式的副本。
