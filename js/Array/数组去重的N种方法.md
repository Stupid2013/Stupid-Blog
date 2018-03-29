### 数组去重的N种方法

#### 一、简单数组去重

#### 二、子元素为对象的数组去重
原数组：
```javascript
const students = [{
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
1. `splice()`方法
代码如下：
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

2. `push()`方法
代码如下：
```javascript
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
```
原理解析：在`for`循环外定义一个空数组，用来装排序后的数组，然后在一层循环里定义一个`index`用于计算数组`arr`里已经存在的该元素的数量，如果数量不为0，则放进去，否则不做任何处理。

注： 上面两种方法来自于我鹏哥～～在此学习记录一下～～～✌️。而且上面两种方法都只用了数组里最基本的两种方法，所以在低版本`IE`里兼容性也是杠杠的！当然，这些方法也适用于简单数组的去重。
