# JavaScript this指向

## this指向注意事项：

- this，谁调用指谁
- 构造函数的优先级(访问权限) > 原型链
- new的时候，this就会指向new出来的对象
- 箭头函数 
  - 箭头函数相当于bind，会改变this的指向
  - 箭头函数的this是他父级的this
  - 箭头函数把this固定到他父级的同级作用
- 能改变this指向的关键词 
  - bind
  - apply
  - call
- 一般函数this指向在执行的时候绑定
- 箭头函数中的this是在定义函数的时候绑定
- 严格模式 
  - 非严格模式下默认指向window；
  - 严格模式下传null指向null，不传或者传undefined都指向undefined，即在严格模式下，没有写执行主体，this指向是undefined。

### 1-1 示例：

```javascript
this.a = 20;
function test() {
    console.log(this.a);
}
test();
```

**答案：** 20

**解析：** 谁调用指向谁，这里指window，即全局：

```javascript
this.a = 20;
function test() {
    console.log(this.a);
}
// 相当于window.test();  window调用，指向window
test();  // 20
```

### 1-2 示例：

```javascript
this.a = 20;
var test = {
    a: 40,
    init: function () {
        console.log(this.a);
    }
}
test.init();
```

**答案：** 40

**解析：**  test调用init方法，this指向test：

```javascript
this.a = 20;
var test = {
    a: 40,
    init: function () {
        console.log(this.a);
    }
}
// test调用init方法，this指向test
test.init();  // 40
```

### 1-3 示例：

```javascript
this.a = 20;
var test = {
    a: 40,
    init: function () {
        console.log(this.a);
    }
}
var fn = test.init;
fn();
```

**答案：** 20

**解析：**  相当于window.fn，这里的this指向window：

```javascript
this.a = 20;
var test = {
    a: 40,
    init: function () {
        console.log(this.a);
    }
}
var fn = test.init;
// 相当于window.fn，这里的this指向window
fn();  // 20
```

### 1-4 示例：

```javascript
this.a = 20;
var test = {
    a: 40,
    init: function () {
        function go() {
            console.log(this.a);
        }
        go();
    }
}
test.init();
```

**答案：** 20

**解析：**  没有人调用go，相当于window.go，this指全局

```javascript
this.a = 20;
var test = {
    a: 40,
    init: function () {
        function go() {
            console.log(this.a);
        }
        // 相当于window.go
        go();  // 20
    }
}
test.init();
```

### 1-5 示例：

```javascript
this.a = 20;
var test = {
    a: 40,
    init: function () {
        function go() {
            this.a = 50;
        }
        go.prototype.a = 60;
        console.log((new go()).a);
    }
}
test.init();
```

**答案：** 50

**解析：** 构造函数的优先级(访问权限) > 原型链，并且用new调用go，this就指向实例，此时的this指向就是go，而构造方法的this指向优先级高，所以a就是在go的实例的构造方法中的值。

```javascript
this.a = 20;
var test = {
    a: 40,
    
    // 构造函数的优先级(访问权限) > 原型链
    init: function () {
        // 构造方法
        function go() {
            this.a = 50;
        }

        // 原型链
        go.prototype.a = 60;

        // new的时候，this就会指向new出来的对象
        // 用new调go，this就指向实例
        console.log((new go()).a);
    }
}
test.init();  // 50
```

### 1-6 示例：

```javascript
this.a = 20;
var test = {
    a: 40,

    init: () => {
        console.log(this.a);
    }
}
test.init();
```

**答案：** 20

**解析：**  注意：箭头函数中的this是在定义函数的时候绑定，而不是在执行函数的时候绑定，所谓的定义时候绑定，就是this是继承自父执行上下文！！ 
 箭头函数相当于bind，会改变this的指向，它把这个this固定到他父级的同级作用域中了，即箭头函数本身与init平级，也就是箭头函数本身所在的对象为test，而test的父执行上下文就是window，所以this指向在全局中。

```javascript
this.a = 20;
var test = {
    a: 40,
    /**
     * 箭头函数相当于bind，会改变this的指向
     * 箭头函数的this是他父级的this
     * 箭头函数把this固定到他父级的同级作用域去
     */
    init: () => {
        console.log(this.a);
    }
}
test.init();  // 20
```

### 1-7 示例：

```javascript
function fn() {
    console.log(this.length);
}
fn();  // iframe数量
```

**答案：** 0

**解析：**  在全局中，this的length指的是iframe的数量

```javascript
function fn() {
    console.log(this.length);
}
fn();  // 0 => iframe数量
```