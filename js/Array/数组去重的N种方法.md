### 数组去重的N种方法

#### 一、简单数组去重
原数组:
```javascript
const array = ['abc','abc','sss','2','d','t','2','sss', 'ss', 'f', 'a', 'A', 'A', 'B', '22','d'];
```

1. `indexOf()`和`push()`
```javascript
const arr = [];
for(let i = 0;i < array.length; i++){
  if(arr.indexOf(array[i]) == -1){
    arr.push(array[i]);
  }
}
console.log('=== arr ', arr); // === arr ["abc", "sss", "2", "d", "t", "ss", "f", "a", "A", "B", "22"]
```

2. 利用`{}`
```javascript
const arr = [];
const obj = {};
for (let i = 0; i < array.length; i++) {
  if (!obj[array[i]]) {
    obj[array[i]] = true;
    arr.push(array[i]);
  }
}
console.log('=== arr ', arr);
```

上面的写法有一个弊端，当数组中存在`[2, '2']`这种情况时，结果只会返回`[2]`，很显然，它们是不同的数据类型，应该都返回才对，下面的写法就会返回`[2, '2']`，至于具体使用哪种方法，就看需求而定啦。

高端写法：利用数组的`prototype`来扩展实现，当然，本文中所有的方法都可以这样封装，在项目中，多次使用的时候可以这样玩儿比较方便。
```javascript
Array.prototype.unique = function() {
  var json = {};
  var result = [];
  this.forEach(function(value){
    var type = Object.prototype.toString.call(value).match(/\s(\w+)/)[1].toLowerCase();
    if(!((type + '-'+value) in json)){ // 对元素类型的判断
      json[type + '-'+value] = true;
      result.push(value);
    }
  })
  return result;
}
```

3. `forEach()`、`indexOf()`和`push()`
```javascript
// 方法一
const arr = [];
array.forEach((e, i, item) => { // 也可以用map()和filter()方法实现
  if (item.indexOf(e) === i) {
    arr.push(e);
  }
});
console.log('=== arr ', arr);

// 方法二
const arr = [];
array.forEach((item, i, ar) => {  //这里利用map，filter方法也可以实现
  if(ar.indexOf(item,i+1) === -1){ //从传入参数的下一个索引值开始寻找是否存在重复
    arr.push(item);
  }
})
console.log('=== arr ', arr);
// 其实原理和方法一好像是一样的。。。。阿西吧，没理解到，理解到再追
```

4. `filter()`和`indexOf()`
```javascript
const arr = array.filter((m, n) => (array.indexOf(m) == n));
console.log('=== arr ', arr);
```

5. `filter()`
```javascript
Array.prototype.unique = function() {
  var sortArr = this.sort();
  return sortArr.filter(function(v,i,context){
    return v !== context[i+1];
  })
}
```

6. `splice()`和`sort()`实现数组递归去重
```javascript
array.sort();
const loop = (index) => {
  if(index >= 1){
   if(array[index] === array[index-1]){
    array.splice(index,1);
   }
   loop(index - 1); //递归loop函数
  }
}
loop(array.length - 1);
console.log('=== array ', array);
```

7. `splice()`
```javascript
Array.prototype.unique = function() {
  var sortArr = this.sort(),
    i = 0;
  for(; i < sortArr.length; i++){
    if(sortArr[i] === sortArr[i+1]){
      sortArr.splice(i,1);
      i--;
    }
  }
  return sortArr;
}
```

8. `Set()`
```javascript
Array.prototype.unique = function() {
  return [...new Set(this)];
}
```

9. `Set()`和`Array.from()`

```javascript
Array.prototype.unique = function() {
  return Array.from(new Set(this));
}
```

10. `includes()`
```javascript
Array.prototype.unique = function() {
  var result = [];
  this.forEach(function(v){
    if(!result.includes(v)){
      result.push(v);
    }
  })
  return result;
}
```

11. `reduce()`
```javascript
Array.prototype.unique = function() {
  var sortArr = this.sort(), result = [];
  sortArr.reduce((v1,v2) => {
    if(v1 !== v2){
      result.push(v1);
    }
    return v2;
  })
  result.push(sortArr[sortArr.length - 1]);
  return result;
}
```

#### 二、子元素为对象的数组去重
原数组:
```javascript
let students = [{
  name: '张三',
  serial: '0001'
}, {
  name: '李四',
  serial: '0002'
}, {
  name: '王五',
  serial: '0003'
}, {
  name: '王五2',
  serial: '0003'
}, {
  name: '赵四',
  serial: '0004'
}, {
  name: '小明',
  serial: '005'
}, {
  name: '小张',
  serial: '006'
}, {
  name: '小李',
  serial: '006'
}, {
  name: '小李2',
  serial: '006'
}, {
  name: '赵四2',
  serial: '0004'
}];
```
要求: 根据`serial`去重。
1. `splice()`
```javascript
for (let i = 0; i < students.length; i++) {
  let repeat = 0;
  for (let j = 0; j < students.length; j++) {
    if (students[i].serial === students[j].serial) {
      repeat++;
      if (repeat > 1) {
        students.splice(j, 1);
      }
    }
  }
}
```
原理解析：一层循环里使用`repeat`计算对象里`serial`相同的数量，如果数量大于1，则把后面的删掉。

2. `push()`
```javascript
// 方法一
const arr = [];
for (let i = 0, l = students.length; i < l; i++) { // let i = 0, l = students.length; i < l; i++这样的写法效率高于i<students.length
  let index = 0;
  for (let j = 0, length = arr.length; j < length; j++) {
    if (students[i].serial === arr[j].serial) {
      index++;
    }
  }
  if (index === 0) {
    arr.push(students[i]);
  }
}
console.log('=== arr ', arr);
// 原理解析: 在`for`循环外定义一个空数组，用来装排序后的数组，然后在一层循环里定义一个`index`用于计算数组`arr`里已经存在的该元素的数量，如果数量不为0，则放进去，否则不做任何处理。

// 方法二
const arr = [];
for(let i = 0; i < array.length; i++) {
  for(let j = 0; j < array.length; j++) {
    if(array[i] == arr[j]) {
        break;
    }
  }
  if(array[i] != arr[j]) {  
    arr.push(array[i]);
  }
}
console.log('=== arr ', arr);
```
注: `splice()`和`push()`是最基本的两种方法，所以在低版本`IE`里兼容性也是杠杠的！

3. `reduce()`
```javascript
const obj = {};
students = students.reduce((cur,next) => {
    obj[next.serial] ? "" : obj[next.serial] = true && cur.push(next);
    return cur;
},[]) //设置cur默认类型为数组，并且初始值为空的数组
console.log('== students ', students);
```

注：文中提到的方法其实基本上都是可以在简单数组和对象数组中实现去重的~~~

说明： 文中部分代码参考自网上的各种代码，比较多，也比较杂，就不一一列举了。。。你一搜，就都出来啦😂
