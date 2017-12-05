## forEach()和 map()的区别以及如何兼容IE8

### 一、区别和共同点

1. 共同点：forEach() 和 map() 里面每一次执行匿名函数都支持3个参数: item(每一项的值), index(索引), array(原始数组)。

2. 区别：forEach()没有返回值, map()有返回值。


* forEach()

```javascript
  arr[].forEach(function(value,index,array){
  　　//do something
}, param2);
```
理论上这个方法是没有返回值的，仅仅是遍历数组中的每一项，不对原来数组进行修改；但是可以自己通过数组的索引来修改原来的数组.

```javascript
  var arr = [12,23,24,42,1];
  var res = arr.forEach(function (item,index,input) {
    input[index] = item*10;
  })
  console.log(res); //--> undefined;
  console.log(arr); //--> [120,230,240,420,10]  原来的数组被改变；
```
forEach()的第二个参数改变回调函数里的this指向。**map()的第二个参数也会改变this指向！！！同forEach()**
```javascript
  var arr = [1,2,3,4,5];
  var arr2 = ["a","b","c","d","e"];
  arr.forEach(function(value, index, array){
    console.log(`第${index}的值是：` + value);
    console.log(this);          // 第二个参数改变回调函数里面的this指向  this = ["a", "b", "c", "d", "e"];
    console.log(`${index} `, this[index]);  // 会打印 a, b, c, d, e
  }, arr2);
```

* map()

```javascript
arr[].map(function(value,index,array){
　　//do something
　　return XXX
}, param2)
```
map()有返回值，可以return。 返回值返回的是当前项修改以后的值，但是不是修改的原数组中的值，而是修改的原数组的拷贝。

```javascript
  var arr = [12,23,24,42,1];
  var res = arr.map(function (item,index,input) {
    return item*10;
  })
  console.log(res); //--> [120,230,240,420,10]
  console.log(arr); //--> [12,23,24,42,1]
```

### 二、兼容IE8及以下

不管是forEach还是map在IE6-8下都不兼容, 那么需要我们自己封装一个都兼容的方法:

1. forEach()

```javascript
  /**
  * forEach遍历数组
  * @param callback [function] 回调函数；
  * @param context [object] 上下文；
  */
  Array.prototype.myForEach = function myForEach(callback,context){
      context = context || window;
      //IE6-8下自己编写回调函数执行的逻辑
      for(var i = 0,len = this.length; i < len;i++) {
          callback && callback.call(context,this[i],i,this);
      }
  }

  if (!Array.prototype.forEach) {  // Array.prototype.forEach不存在时再使用自定义的
    [1,3,2,5].myForEach((item, index) => {
      console.log(item); //--> 1 3 2 5
    });
  }
```

2. map()

```javascript
  /**
  * map遍历数组
  * @param callback [function] 回调函数；
  * @param context [object] 上下文；
  */
  Array.prototype.myMap = function myMap(callback,context){
      context = context || window;
      if(Array.prototype.map) {
          return this.map(callback,context);
      }
      //IE6-8下自己编写回调函数执行的逻辑
      var newAry = [];
      for(var i = 0,len = this.length; i < len;i++) {
          if(typeof  callback === 'function') {
              var val = callback.call(context,this[i],i,this);
              newAry[newAry.length] = val;
          }
      }
      return newAry;
  }

  const arr = [1,3,2,5];
  const trans = arr.myMap(item => item*2);
  console.log(trans); //--> [2,6,4,10]
```
