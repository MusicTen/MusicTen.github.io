# JavaScript 字符串操作

## JavaScript生成6位随机数验证码

```javascript
Math.random().toString().slice(-6)
// Math.random方法 用于生成0~1之间的随机数
// toString方法 用于将生成的随机数转换成字符串
// slice方法 用于截取转换后的字符串，传入参数为负数时代表从字符串尾部开始朝头部方向截取
```

```javascript
Math.random().toString().match(/\d{6}/)[0]
//match() 方法，在字符串内找到相应的值并返回这些值，（）内匹配字符串或者正则表达式。该方法类似 indexOf() 和 lastIndexOf()，但是它返回指定的值，而不是字符串的位置
```

