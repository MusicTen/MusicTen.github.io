# CSS Sass、LESS 和 Stylus

## 一、什么是CSS预处理器

　　CSS预处理器定义了一种新的语言，基本的思想是用一种专门的编程语言，开发者只需要使用这种语言进行编码工作，减少枯燥无味的CSS代码的编写过程的同时，它能让你的CSS具备更加简洁、适应性更强、可读性更加、层级关系更加明显、更易于代码的维护等诸多好处。

　　CSS预处理器种类繁多，本次就以Sass、Less、Stylus进行比较。

## 二、语法

　　首先Sass和Less都是用的是标准的CSS语法，因此你可以很方便的把已完成的CSS代码转为预处理器识别的代码，**Sass**默认使用 `.sass`扩展名，而**Less**默认使用`.Less`扩展名。

less只支持标准语法：

```less
/* style.less */
h1 {
  color: #0982C1;
}
```

Sass同时可支持老语法，就是不包含花括号和分号的书写格式：

```scss
/* style.sass */
h1
  color: #0982c1
```

Stylus支持的语法就更多样性，它默认使用`.styl`的文件扩展名，下面是Stylus语法：

```stylus
h1
  color #0982C1
```

## 三、变量

　　你可以在CSS预处理中声明变量，并在整个样式单中使用，支持任何类型的变量，例如：颜色、数值（是否包含单位）、文本。然后你可以任意的调取和使用该变量。**Sass的变量是必须$开始**，然后变量名和值要冒号隔开，跟CSS属性书写格式一致。

```scss
$mainColor: #0982c1;
$siteWidth: 1024px;
$borderStyle: dotted;
 
body {
  color: $publicColor;
  border: 1px $borderStyle $mainColor;
  max-width: $siteWidth;
}
```

而**Less的变量名使用@符号开始**：

```less
@mainColor: #0982c1;
@siteWidth: 1024px;
@borderStyle: dotted;
 
body {
  color: @publicColor;
  border: 1px @borderStyle @mainColor;
  max-width: @siteWidth;
}
```

　　**Stylus对变量名没有任何限定**，你可以是$开始，也可以是任意字符，而且与变量值之间可以用冒号、空格隔开，需要注意的是 Stylus (0.22.4) 将会编译 @ 开始的变量，但其对应的值并不会赋予该变量，换句话说，在 Stylus 的变量名不要用 @ 开头。

```stylus
mainColor = #0982c1
siteWidth = 1024px
$borderStyle = dotted

body
  color mainColor
  border 1px $borderStyle mainColor
  max-width siteWidth
```

上面三种不同的CSS写法，最终将会生成相同结果：

```css
body {
  color: #0982c1;
  border: 1px dotted #0982c1;
  max-width: 1024px;
}
```

　　最容易体现它的好处的是，假设你在CSS中使用同一种颜色数十次，如果你要修改显色，需要找到并修改十次相同的代码，而有了CSS预处理器，修改一个地方就够了。

## 四、嵌套

　　如果你需要在CSS中相同的parent引用多个元素，你需要一遍一遍的去写parent。例如：

```css
section {
  margin: 10px;
}
section nav {
  height: 25px;
}
section nav a {
  color: #0982C1;
}
section nav a:hover {
  text-decoration: underline;
}
```

然而如果用CSS预处理器，就可以少些很多单词，而且父节点关系一目了然。

```css
section {
  margin: 10px;
 
  nav {
    height: 25px;
 
    a {
      color: #0982C1;
 
      &amp;:hover {
        text-decoration: underline;
      }
    }
  }
}
```

stylus还可省略花括号，书写更加方便，根据个人喜好还可删除中间冒号。

```stylus
section 
  margin: 10px;
  nav 
    height: 25px;
    a 
      color: #0982C1;
      &amp;:hover 
        text-decoration: underline;
```

最终生成CSS结果是：

```css
section {
  margin: 10px;
}
section nav {
  height: 25px;
}
section nav a {
  color: #0982C1;
}
section nav a:hover {
  text-decoration: underline;
}
```

## 五、运算符

在 CSS 预处理器中还是可以进行样式的计算如下：

```css
body {
  margin: (14px/2);
  top: 50px + 100px;
  right: 80 * 10%;
}
```

在sass，Less与stylus中都是可以这样做的。

## 六、颜色函数

CSS 预处理器一般都会内置一些颜色处理函数用来对颜色值进行处理，例如加亮、变暗、颜色梯度等。

1.sass的颜色处理函数：

```scss
lighten($color, 10%);
darken($color, 10%);
saturate($color, 10%); 
desaturate($color, 10%);
grayscale($color);
complement($color);
invert($color);
mix($color1, $color2, 50%);
```

实例如下：

```scss
$color: #0982C1;
h1 {
	background: $color;
	border: 3px solid darken($color, 50%);
}
```

2.Less css颜色处理函数：

```less
lighten(@color, 10%);
darken(@color, 10%);
saturate(@color, 10%);
desaturate(@color, 10%);
spin(@color, 10);
spin(@color, -10);
mix(@color1, @color2);
```


示例如下：

```less
@color: #0982C1;
h1 {
  background: @color;
  border: 3px solid darken(@color, 50%);
}
```

3.Stylus颜色处理函数：

```stylus
lighten(color, 10%);
darken(color, 10%);
saturate(color, 10%);
desaturate(color, 10%);
```


示例如下：

```stylus
color = #0982C1 
h1
  background color
  border 3px solid darken(color, 50%)
```

## 七、导入 (Import)

很多 CSS 开发者对导入的做法都不太感冒，因为它需要多次的 HTTP 请求。但是在 CSS 预处理器中的导入操作则不同，它只是在语义上包含了不同的文件，但最终结果是一个单一的 CSS 文件，如果你是通过 `@ import “file.css”; `导入 CSS 文件，那效果跟普通的 CSS 导入一样。

注意：导入文件中定义的混入、变量等信息也将会被引入到主样式文件中，因此需要避免它们互相冲突。 
例如： 
1.css:

```css
//1.css
/* file.{type} */
body {
  background: #000;
}
```

2.XXX:

```css
@ import "1.css";
@ import "file.{type}";

p {
  background: #092873;
}
```

最终生成的 CSS：

```css
@ import "1.css";
body {
  background: #000;
}
p {
  background: #092873;
}
```

## 八、继承

当我们需要为多个元素定义相同样式的时候，我们可以考虑使用继承的做法.

1.sass： 
sass可通过`@extend`来实现代码组合声明，使代码更加优越简洁。

```scss
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}
.success {
  @extend .message;
  border-color: green;
}
.error {
  @extend .message;
  border-color: red;
}
.warning {
  @extend .message;
  border-color: yellow;
}
```

2.Less css：

但是在这方面 Less 表现的稍微弱一些，更像是混入写法：

```less
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}
.success {
  .message;
  border-color: green;
}
.error {
  .message;
  border-color: red;
}
.warning {
  .message;
  border-color: yellow;
}
```

3.Stylus的@extend指令受`SASS实现`的启发，基本一致，除了些轻微差异。

上面两种写法其最终呈现的css样式都如下：

```css
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}
.success {
  border-color: green;
}
.error {
  border-color: red;
}
.warning {
  border-color: yellow;
}
```


.message的样式将会被插入到相应的你想要继承的选择器中，但需要注意的是优先级的问题。

## 九、Mixins（混入）

Mixins 有点像是函数或者是宏，当某段 CSS 经常需要在多个元素中使用时，可以为这些共用的 CSS 定义一个 Mixin，然后只需要在需要引用这些 CSS 地方调用该 Mixin 即可。

1.Sass 的混入语法：

sass中可用mixin定义一些代码片段，且可传参数，方便日后根据需求调用。比如说处理css3浏览器前缀：

```scss
@mixin error($borderWidth: 2px) {
  border: $borderWidth solid #F00;
  color: #F00;
}
.generic-error {
  padding: 20px;
  margin: 4px;
  @ include error(); //这里调用默认 border: 2px solid #F00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  @ include error(5px); //这里调用 border:5px solid #F00;
}
```

2.Less CSS 的混入语法： 
less也支持带参数的混合以及有默认参数值的混合，如下面的例子所示：

```less
.error(@borderWidth: 2px) {
  border: @borderWidth solid #F00;
  color: #F00;
}
.generic-error {
  padding: 20px;
  margin: 4px;
  .error(); //这里调用默认 border: 2px solid #F00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  .error(5px); //这里调用 border:5px solid #F00;
}
```

3.Stylus 的混入语法：

```stylus
error(borderWidth= 2px) {
  border: borderWidth solid #F00;
  color: #F00;
}
.generic-error {
  padding: 20px;
  margin: 4px;
  error(); 
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  error(5px); 
}
```


他们最终呈现的效果都如下：

```css
.generic-error {
  padding: 20px;
  margin: 4px;
  border: 2px solid #f00;
  color: #f00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  border: 5px solid #f00;
  color: #f00;
}
```