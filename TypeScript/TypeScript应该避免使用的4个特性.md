# TypeScript应该避免使用的4个特性

## 1.避免使用枚举类型enum

枚举为一组常量命名。 在下面的示例中，HttpMethod.Get 是字符串“GET”的名称。 HttpMethod 类型在概念上类似于文字类型之间的联合类型，例如 'GET' | 'POST'。

```js
enum HttpMethod {
  Get = 'GET',
  Post = 'POST',
}
const method: HttpMethod = HttpMethod.Post;
method; // Evaluates to 'POST'

```

枚举的缺点在于它们如何适应 TypeScript 语言。 TypeScript 基于JavaScript具有静态类型功能的语言，本质上还是js。如果我们从 TypeScript 代码中删除所有类型，剩下的应该是有效的 JavaScript 代码。 TypeScript 文档中使用的正式词是“类型级扩展”：大多数 TypeScript 功能都是对 JavaScript 的类型级扩展，它们不会影响代码的运行时行为。如如下代码：

```js
function add(x: number, y: number): number {
  return x + y;
}
add(1, 2); // Evaluates to 3

```

编译器会检查代码类型，然后去除类型生成js代码，即将类型number删除，剩下的就是javascript代码,如下：

```
function add(x, y) {
  return x + y;
}
add(1, 2); // Evaluates to 3

```

大多数 TypeScript 功能都以这种方式工作，遵循类型级扩展规则。 要获取 JavaScript 代码，编译器只需删除类型注释。但是枚举打破了这个规则。 HttpMethod 和 HttpMethod.Post 是类型的一部分，因此在 TypeScript 生成 JavaScript 代码时应该将它们删除。即使编译器只是从上面的代码示例中删除枚举类型，仍然会留下引用 HttpMethod.Post 的 JavaScript 代码， 那会在执行过程中出错：如果编译器删除了HttpMethod.Post，就不能引用它！

```js
/* 这是引用 TypeScript 枚举的编译 JavaScript 代码。 但如果
 * TypeScript 编译器只是删除了枚举，然后就没有什么可做的了
 * reference!
 *
 * 此代码在运行时失败：
 *   Uncaught ReferenceError: HttpMethod is not defined */
const method = HttpMethod.Post;

```

### 1.1.原因

TypeScript 在这种情况下的解决方案是打破自己的规则(**类型级扩展规则**)。编译枚举时，编译器会发出原始 TypeScript 代码中不存在的额外 JavaScript 代码。很少有 TypeScript 功能可以像这样工作，并且每个功能都给原本简单的 TypeScript 编译器模型增加了令人困惑的复杂性。由于这些原因，我们建议避免使用枚举并使用联合。TypeScript 在这种情况下的解决方案是打破自己的规则。编译枚举时，编译器会发出原始 TypeScript 代码中不存在的额外 JavaScript 代码。很少有 TypeScript 功能可以像这样工作，并且每个功能都给原本简单的 TypeScript 编译器模型增加了令人困惑的复杂性。由于这些原因，我们建议避免使用枚举并使用联合。

### 1.2.为什么类型级扩展规则很重要？

​	让我们考虑一下规则如何与 JavaScript 和 TypeScript 工具的生态系统交互。 TypeScript 项目本质上是 JavaScript 项目，因此它们经常使用 Babel 和 webpack 等 JavaScript 构建工具。这些工具是为 JavaScript 设计的，今天仍然是他们的主要关注点。每个工具也是它自己的生态系统。有一个看似无穷无尽的 Babel 和 webpack 插件来处理代码。Babel、webpack、它们的众多插件以及生态系统中的所有其他工具和插件如何完全支持 TypeScript？对于大多数 TypeScript 语言，类型级扩展规则使这些工具的工作相对容易。这些工具去除了类型注释，留下了有效的 JavaScript。当涉及到枚举（和命名空间）时，事情就更加困难了。仅仅删除枚举是不够的。这些工具必须将 enum HttpMethod { ... } 转换为有效的 JavaScript 代码，即使 JavaScript 根本没有枚举。这带来了 TypeScript 违反其自身**类型级扩展规则**的实际问题。像 Babel、webpack 和它们的插件这样的工具都是首先为 JavaScript 设计的，所以 TypeScript 支持只是它们众多特性之一。有时，TypeScript 支持不像 JavaScript 支持那样受到关注，这可能会导致错误。绝大多数工具在变量声明、函数定义等方面都会做得很好；所有这些都相对容易使用。但有时错误会随着枚举和命名空间蔓延，因为它们需要的不仅仅是剥离类型注释。TypeScript 编译器本身可以正确编译这些功能，但生态系统中一些很少使用的工具可能会出错。当编译器、捆绑程序、压缩程序、linter、代码格式化程序等默默地错误编译或误解了系统的一个偏僻部分时，调试可能会非常困难。众所周知，编译器错误很难追踪。

## 2.避免使用命名空间namespace

命名空间类似于模块，不同之处在于多个命名空间可以存在于一个文件中。 例如，我们可能有一个文件，为它的导出代码和它的测试定义了单独的命名空间。 （不建议这样做）

```js
namespace Util {
  export function wordCount(s: string) {
    return s.split(/\b\w+\b/g).length - 1;
  }
}
namespace Tests {
  export function testWordCount() {
    if (Util.wordCount('hello there') !== 2) {
      throw new Error("Expected word count for 'hello there' to be 2");
    }
  }
}
Tests.testWordCount();

```

​	命名空间在实践中可能会导致问题。在上面关于枚举的部分，看到了 TypeScript 的“类型级扩展”规则。通常，编译器会删除所有类型注释，剩下的是有效的 JavaScript 代码。

### 2.1.原因

命名空间以与枚举相同的方式打破类型级扩展规则。在命名空间 Util { export function wordCount ... } 中，无法删除类型定义。整个命名空间是一个特定于 TypeScript 的类型定义！调用 Util.wordCount(...) 的命名空间之外的其他代码会发生什么情况？如果在生成 JavaScript 代码之前删除 Util 命名空间，那么 Util 将不再存在，因此 Util.wordCount(...) 函数调用不可能工作。**与枚举一样，TypeScript 编译器不能简单地删除命名空间定义。相反，它必须生成原始 TypeScript 代码中不存在的新 JavaScript 代码，这样反而会影响编译性能** 

- 对于枚举，建议是使用联合。
- 对于命名空间，建议使用常规模块。
- 创建许多小文件可能有点烦人，但模块具有与命名空间相同的基本功能，没有潜在的缺点。

## 3.避免使用装饰器decorator

装饰器是修改或替换其他函数（或类）的函数。

```js
// This is the decorator.
@sealed
class BugReport {
  type = "report";
  title: string;
  constructor(t: string) {
    this.title = t;
  }
}

```

上面的@sealed 装饰器暗示了C# 的sealed 修饰符，它可以防止其他类从sealed 类继承.。通过编写一个密封函数来实现它，该函数接受一个类并修改它以防止继承。

### 3.1.原因

装饰器首先被添加到 TypeScript 中，然后才开始使用 JavaScript (ECMAScript) 进行标准化。**截至 2022 年 1 月，装饰器仍然是 ECMAScript 的第 2 阶段提案。第二阶段是“草稿”。装饰者提案似乎也陷入了委员会的炼狱：自 2019 年 2 月以来一直处于第 2 阶段。** 建议避免使用装饰器，直到它们至少是第 3 阶段（“候选”）提案，或者对于更保守的团队来说是第 4 阶段（“完成”）。但ECMAScript 装饰器总是有可能永远不会最终确定。如果发生这种情况，它们最终会陷入与 TypeScript 枚举和命名空间类似的情况。它们将继续永远破坏 TypeScript 的类型级扩展规则，并且在使用官方 TypeScript 编译器以外的构建工具时，它们更有可能破坏。

## 4.避免使用private关键字

​	TypeScript 有两种方法可以将类字段设为私有。 **有旧的 private 关键字**，它特定于 TypeScript。 然后是**新的#somePrivateField 语法**，它取自 JavaScript。示例：

```js
class MyClass {
  private field1: string;
  #field2: string;
  ...
}

```

推荐新的#somePrivateField 语法的原因很简单：**这两个功能大致相同。 最好希望保持与 JavaScript 的功能对等，减少编译器的转换，提高编译器的转换效率。**

## 5.总结

- 避免枚举，使用联合类型。
- 避免命名空间，使用小文件模块化。
- 使用#somePrivateField 而不是私有的 somePrivateField。
- 推迟使用装饰器，直到它们标准化。