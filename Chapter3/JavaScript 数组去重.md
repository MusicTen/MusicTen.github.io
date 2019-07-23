# JavaScript之数组去重

  平时工作当中经常会遇到数组去重的需求，所以在这里整理一下有哪些方法可以实现数组的去重。

## 1、双层for循环

```javascript
function removeSameEle(arr) {
	let res = [];
	for(var i = 0; i < arr.length; i++) {
		for(var j = 0; j < res.length; j++) {
			if(arr[i] === arr[j]){ // 判断结果中是否已存在该数据了
				break;
			}
		}
	if(j === res.length){
		res.push(arr[i]);
	}
	return res;
}
```

## 2、indexof或者includes

```javascript
function removeSameEle(arr) {
	let res = [];
	for(var i = 0; i < arr.length; i++) {
		//if(!res.includes(arr[i])){
		if(res.indexof(arr[i]) < 0) { // 判断结果中是否存在该数据了
			res.push(arr[i])
		}
	}
	return res;
}
```

## 3、filter

```javascript
function removeSameEle(arr) {
    let res = arr.filter((item, index, arr) => {
        return arr.indexOf(item) === index;
    });
    return res;
}
```

## 4、set唯一值去重

  此方法只能用于对简单数组进行去重。

```javascript
function removeSameEle(arr) {
    let res = new Set(arr);
    return [...res];
}
```

## 5、sort排序后去重

```javascript
function removeSameEle(arr) {
    let res = [];
    let sortedArr=arr.sort();
    for(let i = 0; i < sortedArr.length - 1; i++){
        if(i == 0 || sortedArr[i] !== sortedArr[i + 1]) {
            res.push(sortedArr[i]);
            if(i === (sortedArr.length - 2)){
                res.push(sortedArr[i + 1]);
            }
        }
    }
    return res;
}
```

## 6、使用object.keys()去重

```javascript
function removeSameEle(arr) {
    let res = [];
    let obj = {};
    arr.forEach((item,index) => {
        if(!obj[item]) {
            obj[item] = item；
        }
    });
    for(let value of Object.values(obj)) {
        res.push(value);
    }
    return res;
}
```