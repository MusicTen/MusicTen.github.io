##  JavaScript 数组操作

### 方法

- **toString()** - 将数组转换为以逗号分隔的字符串。
- **join()** - 将所有数组元素组合成一个字符串。
- **concat** - 将两个数组组合在一起，或者将更多项添加到数组中，然后返回一个新数组。
- **push()** - 将项目添加到数组的末尾，**改变**原始数组。
- **pop()** - 删除数组的最后一项并**返回**
- **shift()** - 删除数组的第一项并**返回**
- **unshift()** - 将一个项添加到数组的开头，**改变**原始数组。
- **splice()** - 通过添加，删除和插入元素**修改**一个数组。
- **slice()** - 复制数组的给定部分，并将复制的部分作为新数组返回。 **它不会改变原始数组。**
- **split()** - 将一个字符串分成子串并将它们作为数组返回。
- **indexOf()** - 查找数组中的项目并返回其**索引**，如果没找到则返回`-1`
- **lastIndexOf()** - 从右到左查找项目并返回找到的最后一个索引。
- **filter()** - 如果数组的项目符合某个条件，则创建一个新数组。
- **map()** - 通过操纵数组中的值来创建一个新数组。
- **reduce()**  - 根据数组中的单个值进行计算， 包含四个参数。
- **forEach()** - 遍历数组，将函数作用于数组中的所有项
- **every()** - 检查数组中的所有项是否都符合指定的条件，如果符合则返回 `true`，否则返回 `false`。
- **some()** - 检查数组中的项（一个或多个）是否符合指定的条件，如果符合则返回 true，否则返回 false。
- **includes()** - 检查数组是否包含某个项目。



### 从数组中获取最大值和最小值

可以使用`Math.max`和`Math.min`取出数组中的最大小值和最小值：

```javascript
const numbers = [15, 80, -9, 90, -99]
const maxInNumbers = Math.max.apply(Math, numbers)
const minInNumbers = Math.min.apply(Math, numbers)

console.log(maxInNumbers)
// Result: 90

console.log(minInNumbers)
// Result: -99
```

另外还可以使用ES6的`...`运算符(展开运算符)来完成：

```javascript
const numbers = [1, 2, 3, 4];
Math.max(...numbers) 
// Result: 4

Math.min(...numbers) 
// Result: 1
```

