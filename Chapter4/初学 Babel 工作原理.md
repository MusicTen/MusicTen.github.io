# 初学 Babel 工作原理

## 前言

`Babel` 对于前端开发者来说应该是很熟悉了，日常开发中基本上是离不开它的。

已经9102了，我们已经能够熟练地使用 `es2015+` 的语法。但是对于浏览器来说，可能和它们还不够熟悉，我们得让浏览器理解它们，这就需要 `Babel`。

当然，仅仅是 `Babel` 是不够的，还需要 `polyfill` 等等，这里就先不说了。

## What：什么是 `Babel`

> Babel is a toolchain that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments.

简单地说，`Babel` 能够转译 `ECMAScript 2015+` 的代码，使它在旧的浏览器（环境）中也能够运行。

我们可以在 <https://babel.docschina.org/repl> 尝试一下。

一个小🌰：

```javascript
// es2015 的 const 和 arrow function
const add = (a, b) => a + b;

// Babel 转译后
var add = function add(a, b) {
  return a + b;
};
```

> `Babel` 的功能很纯粹。我们传递一段源代码给 `Babel`，然后它返回一串新的代码给我们。就是这么简单，它不会运行我们的代码，也不会去打包我们的代码。

**它只是一个编译器**。

## How: Babel 是如何工作的

首先得要先了解一个概念：抽象语法树（Abstract Syntax Tree, AST），`Babel` 本质上就是在操作 `AST` 来完成代码的转译。

我们还是拿上面的🌰来说明 `const add = (a, b) => a + b;`，这样一句简单的代码，我们来看看它生成的 `AST` 会是怎样的：

```json
{
  "type": "Program",
  "body": [
    {
      "type": "VariableDeclaration", // 变量声明
      "declarations": [ // 具体声明
        {
          "type": "VariableDeclarator", // 变量声明
          "id": {
            "type": "Identifier", // 标识符（最基础的）
            "name": "add" // 函数名
          },
          "init": {
            "type": "ArrowFunctionExpression", // 箭头函数
            "id": null,
            "expression": true,
            "generator": false,
            "params": [ // 参数
              {
                "type": "Identifier",
                "name": "a"
              },
              {
                "type": "Identifier",
                "name": "b"
              }
            ],
            "body": { // 函数体
              "type": "BinaryExpression", // 二项式
              "left": { // 二项式左边
                "type": "Identifier",
                "name": "a"
              },
              "operator": "+", // 二项式运算符
              "right": { // 二项式右边
                "type": "Identifier",
                "name": "b"
              }
            }
          }
        }
      ],
      "kind": "const"
    }
  ],
  "sourceType": "module"
}
```

### Babel 工作过程

了解了 `AST` 是什么样的，就可以开始研究 `Babel` 的工作过程了。

上面说过，`Babel` 的功能很纯粹，它只是一个编译器。

大多数编译器的工作过程可以分为三部分：

1. **Parse(解析)**：将源代码转换成更加抽象的表示方法（例如抽象语法树）
2. **Transform(转换)**：对（抽象语法树）做一些特殊处理，让它符合编译器的期望
3. **Generate(代码生成)**：将第二步经过转换过的（抽象语法树）生成新的代码

既然 `Babel` 是一个编译器，当然它的工作过程也是这样的。

