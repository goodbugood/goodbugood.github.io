# 方法


### 判断某个方法是否存在

1). 方法一

```javascript
if (typeof myMethod !== 'undefined' && myMethod instanceof Function) {
	console.log(myMethod, '是方法')
}
```

2). 方法二

```javascript
if (对象名.方法名) {
	
}
```

3). 方法三

```javascript
if (typeof(对象名.方法名) === 'function') {
	// 存在
} else {
	// 不存在
}
```
