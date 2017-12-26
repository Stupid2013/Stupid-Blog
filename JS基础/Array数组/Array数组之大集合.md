## Array数组方法大集合

2017-12-06

在我们的实际开发中，数组可谓是用到的最常见的数据结构了，随着js的发展，它的用法也越来越“高大上”，越来越强大。作为一名小菜鸟，也只对它一些常用的方法比较熟悉，有的时候甚至常用的都不熟悉。。。啊，说来惭愧。因此一直想要总结之，那么，今天！就是这一伟大时刻！！😎

写在前面：数组很多新方法ie低版本并不支持，可以来这儿[MDN Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)去看polyfill😎

集体照：

    valueOf(), toString(), push(), pop(), join(), concat(), shift(), unshift(), reverse(), slice(), splice(), sort()
    es5新增: forEach(), map(), filter(), some(), every(), indexOf(), lastIndexOf(), reduce(), reduceRight()
    es6新增： Array.from(), Array.of(), copyWithin(), find(), findIndex(), fill(), entries(), keys(), values(), includes()
    注：这些都是Array实例的方法。

 **注： Array.isArray(a)可以用来判断a是不是数组。**

接下来对每一个方法的用法详细说明。

### valueOf()和toString()

`valueOf()`返回数组本身，`toString()`返回数组的字符串形式。

```javascript
var a = [1,2,3];
a.valueOf(); //--> [1,2,3]

var b = [1,2,3,[4,5]];
b.toString(); //--> "1,2,3,4,5"
```
### push()

`push()`用于向数组**末端**添加元素，并返回添加新元素后的数组长度。**该方法会改变原数组**

```javascript
var a = [];
a.push(1); // 1
a.push('a'); // 2
a.push(true, {}); // 4
console.log(a); //--> [1, 'a', true, {}]
```
合并两个数组：

```javascript
var a = [1, 2, 3];
var b = [4, 5, 6];

Array.prototype.push.apply(a, b)
// 或者
a.push.apply(a, b)

// 上面两种写法等同于
a.push(4, 5, 6)

a // [1, 2, 3, 4, 5, 6]
```

`push`方法还可以用于向对象添加元素，添加后的对象变成类似数组的对象，即新加入元素的键对应数组的索引，并且对象有一个`length`属性。

```javascript
var a = {a: 1};

[].push.call(a, 2);
a // {0:2, a:1, length: 1}

[].push.call(a, [3]);
a // {0:2, 1:[3], a:1, length: 2}
```

### pop()

`pop()`用于删除数组的最后一个元素，并返回该元素。**该方法会改变原数组。**

```javascript
var a = [1,2,3];
a.pop(); // 3
a // [1,2]
```

注: 对空数组使用`pop`方法，返回`undefined`
```javascript
[].pop() // undefined
```

### join()

`join`方法以参数作为分隔符，将所有数组成员组成一个字符串返回。如果不提供参数，默认用逗号分隔。

```javascript
var a = [1, 2, 3, 4];
a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"
a.join() // "1,2,3,4"
```

如果数组成员是`undefined`或`null`或空位，会被转成空字符串。

```javascript
[undefined, null].join('#')
// '#'

['a',, 'b'].join('-')
// 'a--b'
```

通过`call`方法，这个方法也可以用于字符串。

```javascript
Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"
```

`join()`也可以用于类似数组的对象。

```javascript
var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')
// 'a-b'
```

### concat()

`concat`方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后面，然后返回一个**新数组**(原数组的拷贝)，**原数组不变**。

```javascript
['hello'].concat(['world'])
// ["hello", "world"]

['hello'].concat(['world'], ['!'])
// ["hello", "world", "!"]
```

`concat`也可以接受其他类型的值作为参数。它们会作为新的元素，添加数组尾部。

```javascript
[1, 2, 3].concat(4, 5, 6)
// [1, 2, 3, 4, 5, 6]

// 等同于
[1, 2, 3].concat(4, [5, 6])
[1, 2, 3].concat([4], [5, 6])
```

如果不提供参数，`concat`方法返回当前数组的一个浅拷贝。所谓“浅拷贝”，指的是如果数组成员包括复合类型的值（比如对象），则新数组拷贝的是该值的引用。

```javascript
var obj = { a:1 };  // 换成是数组也是一样的，改变数组的值，就俩都变了 😁
var oldArray = [obj];

var newArray = oldArray.concat();

obj.a = 2;
newArray[0].a // 2
```

上面代码中，原数组包含一个对象，`concat`方法生成的新数组包含这个对象的引用。所以，改变原对象以后，新数组跟着改变。事实上，只要原数组的成员中包含对象，`concat`方法不管有没有参数，总是返回该对象的引用。


`concat`方法也可以用于将对象合并为数组。

```javascript
[].concat({a: 1}, {b: 2})
// [{ a: 1 }, { b: 2 }]

[].concat({a: 1}, [2])
// [{a: 1}, 2]

[2].concat({a: 1})
// [2, {a: 1}]
```

### shift()

`shift`方法用于删除数组的第一个元素，并返回**该元素**。**该方法会改变原数组**

```javascript
var a = ['a', 'b', 'c'];

a.shift() // 'a'
a // ['b', 'c']
```

`shift`方法可以遍历并清空一个数组。

```javascript
var list = [1, 2, 3, 4, 5, 6];
var item;

while (item = list.shift()) {
  console.log(item);
}

list // []
```
### unshift()

`unshift`方法用于在数组的第一个位置添加元素，并返回添加新元素后的**数组长度**。**该方法会改变原数组**

```javascript
var a = ['a', 'b', 'c'];

a.unshift('x'); // 4
a // ['x', 'a', 'b', 'c']
```

`unshift`方法可以在数组头部添加多个元素。

```javascript
var arr = [ 'c', 'd' ];
arr.unshift('a', 'b') // 4
arr // [ 'a', 'b', 'c', 'd' ]
```

### reverse()

`reverse`方法用于颠倒数组中元素的顺序，返回**改变后的数组**。**该方法将改变原数组**

```javascript
var a = ['a', 'b', 'c'];
a.reverse() // ["c", "b", "a"]
a // ["c", "b", "a"]
```

### slice()

`slice`方法用于截取原数组的一部分，返回一个新数组，**原数组不变**。

它的第一个参数为起始位置（从0开始），第二个参数为终止位置（但该位置的元素本身不包括在内）。如果省略第二个参数，则一直返回到原数组的最后一个成员。

```javascript
// 格式
arr.slice(start_index, end_index);

// 用法
var a = ['a', 'b', 'c'];

a.slice(0) // ["a", "b", "c"]
a.slice(1) // ["b", "c"]
a.slice(1, 2) // ["b"]
a.slice(2, 6) // ["c"]
a.slice() // ["a", "b", "c"]
```

上面最后一个例子`slice`没有参数，实际上等于返回一个原数组的拷贝。

如果`slice`方法的参数是负数，则表示倒数计算的位置。

```javascript
var a = ['a', 'b', 'c'];
a.slice(-2) // ["b", "c"]
a.slice(-2, -1) // ["b"]
```

上面代码中，`-2`表示倒数计算的第二个位置，`-1`表示倒数计算的第一个位置。

如果参数值大于数组成员的个数，或者第二个参数小于第一个参数，则返回空数组。

```javascript
var a = ['a', 'b', 'c'];
a.slice(4) // []
a.slice(2, 1) // []
```

`slice`方法的一个重要应用，是将类似数组的对象转为真正的数组。

```javascript
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```

### splice()

`splice`方法用于删除原数组的一部分成员，并可以在被删除的位置添加入新的数组成员，`返回值是被删除的元素`。**该方法会改变原数组**。

`splice`的第一个参数是删除的起始位置，第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。

```javascript
// 格式
arr.splice(index, count_to_remove, addElement1, addElement2, ...);
// 用法
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2) // ["e", "f"]
a // ["a", "b", "c", "d"]

var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2, 1, 2) // ["e", "f"]
a // ["a", "b", "c", "d", 1, 2]
```

起始位置如果是负数，就表示从倒数位置开始删除。

```javascript
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(-4, 2) // ["c", "d"]
```

如果只是单纯地插入元素，`splice`方法的第二个参数可以设为0。

```javascript
var a = [1, 1, 1];
a.splice(1, 0, 2) // []
a // [1, 2, 1, 1]
```

如果只提供第一个参数，等同于将原数组在指定位置拆分成两个数组。

```javascript
var a = [1, 2, 3, 4];
a.splice(2) // [3, 4]
a // [1, 2]
```

### sort()

`sort`方法对数组成员进行排序，默认是按照字典顺序排序。排序后，**原数组将被改变**。

```javascript
['d', 'c', 'b', 'a'].sort()
// ['a', 'b', 'c', 'd']

[4, 3, 2, 1].sort()
// [1, 2, 3, 4]

[11, 101].sort()
// [101, 11]

[10111, 1101, 111].sort()
// [10111, 1101, 111]
```

上面代码的最后两个例子，需要特殊注意。**sort方法不是按照大小排序**，而是按照对应字符串的字典顺序排序。也就是说，数值会被先转成字符串，再按照字典顺序进行比较，所以`101`排在`11`的前面。

如果想让`sort`方法按照自定义方式排序，可以传入一个函数作为参数，表示按照自定义方法进行排序。该函数本身又接受两个参数，表示进行比较的两个元素。如果返回值大于0，表示第一个元素排在第二个元素后面；其他情况下，都是第一个元素排在第二个元素前面。

```javascript
[10111, 1101, 111].sort(function (a, b) {
  return a - b;
})
// [111, 1101, 10111]

[
  { name: "张三", age: 30 },
  { name: "李四", age: 24 },
  { name: "王五", age: 28  }
].sort(function (o1, o2) {
  return o1.age - o2.age;
})
// [
//   { name: "李四", age: 24 },
//   { name: "王五", age: 28  },
//   { name: "张三", age: 30 }
// ]
```

### map()和forEach()

见[Array数组之forEach()和 map()的区别以及如何兼容IE8](./Array数组之forEach()和map()的区别以及如何兼容IE8.md)

### filter()

`filter`过滤掉数组中不满足条件的值，返回一个新数组，**不改变原数组的值**。

```javascript
[1, 2, 3, 4, 5].filter(function (elem) {
  return (elem > 3);
})
// [4, 5]

// 通过filter方法，返回数组arr里面所有布尔值为true的成员
var arr = [0, 1, 'a', false];

arr.filter(Boolean)
// [1, "a"]
```

`filter`方法还可以接受第二个参数，指定测试函数所在的上下文对象（即`this`对象）。

```javascript
var Obj = function () {
  this.MAX = 3;
};

var myFilter = function (item) {
  if (item > this.MAX) {
    return true;
  }
};

var arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, new Obj())
// [8, 4, 9]
```

上面代码中，测试函数`myFilter`内部有`this`对象，它可以被`filter`方法的第二个参数绑定。上例中，`myFilter`的`this`绑定了`Obj`对象的实例，返回大于3的成员。

### some(), every()

都返回布尔值，`some`方法是只要有一个数组成员的返回值是`true`，则整个`some`方法的返回值就是`true`，否则`false`;`every`方法则是所有数组成员的返回值都是`true`，才返回`true`，否则false。**都不改变原数组**。

```javascript
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
// true

var arr = [1, 2, 3, 4, 5];
arr.every(function (elem, index, arr) {
  return elem >= 3;
});
// false
```

**注意**，对于空数组，`some`方法返回`false`，`every`方法返回`true`，**回调函数都不会执行**。

```javascript
function isEven(x) { return x % 2 === 0 };

[].some(isEven) // false
[].every(isEven) // true
```


`some()`和`every()`还可以接受第二个参数，用来绑定函数中的`this`关键字。

### indexOf(), lastIndexOf()

`indexOf()`返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。

```javascript
var a = ['a', 'b', 'c'];

a.indexOf('b') // 1
a.indexOf('y') // -1
```

`indexOf()`还可以接受第二个参数，表示搜索的开始位置。如果第二个参数为负值，表示从倒数的位置开始搜索。

```javascript
['a', 'b', 'c'].indexOf('a', 1) // -1

['a', 'b', 'c'].indexOf('c', -1) // 2
```

`lastIndexOf()`返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。

```javascript
var a = [2, 5, 9, 2];
a.lastIndexOf(2) // 3
a.lastIndexOf(7) // -1
```

**注意**，如果数组中包含`NaN`，这两个方法不适用，即无法确定数组成员是否包含`NaN`。

```javascript
[NaN].indexOf(NaN) // -1
[NaN].lastIndexOf(NaN) // -1
```

因为这两个方法内部，使用严格相等运算符（`===`）进行比较，**而`NaN`是唯一一个不等于自身的值**。

### reduce(), reduceRight()

`reduce`方法和`reduceRight`方法依次处理数组的每个成员，最终累计为一个值。

它们的差别是，`reduce`是从左到右处理，`reduceRight`则是从右到左，其他完全一样。

这两个方法的第一个参数都是一个函数。该函数接受以下四个参数。

    1. 累积变量，默认为数组的第一个成员
    2. 当前变量，默认为数组的第二个成员
    3. 当前位置（从0开始）(可选)
    4. 原数组(可选)

下面的例子求数组成员之和。

```javascript
[1, 2, 3, 4, 5].reduce(function(x, y){
  console.log(x, y)
  return x + y;
});
// 1 2
// 3 3
// 6 4
// 10 5
//最后结果：15
```

利用`reduce`方法，可以写一个数组求和的`sum`方法。

```javascript
Array.prototype.sum = function (){
  return this.reduce(function (partial, value) {
    return partial + value;
  })
};

[3, 4, 5, 6, 10].sum()
// 28
```

如果要对累积变量指定初值，可以把它放在`reduce`方法和`reduceRight`方法的第二个参数。

```javascript
[1, 2, 3, 4, 5].reduce(function(x, y){
  return x + y;
}, 10);
// 25
```

下面是一个`reduceRight`方法的例子。

```javascript
function substract(prev, cur) {
  return prev - cur;
}

[3, 2, 1].reduce(substract) // 0
[3, 2, 1].reduceRight(substract) // -4
```

由于`reduce`方法依次处理每个元素，所以实际上还可以用它来搜索某个元素。比如，下面代码是找出长度最长的数组元素。

```javascript
function findLongest(entries) {
  return entries.reduce(function (longest, entry) {
    return entry.length > longest.length ? entry : longest;
  }, '');
}

findLongest(['aaa', 'bb', 'c']) // "aaa"
```

### Array.from()

`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象和可遍历的对象（包括 ES6 新增的数据结构`Set`和`Map`）。

```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3  // 决定了返回的数组长度
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
或者 var arr1 = Array.prototype.slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']

let arrayLike2 = {
  '0': 'a',
  '1': 'b',
  length: 3,
}

let arr = Array.from(arrayLike2); // ["a", "b", undefined]
```
实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的`arguments`对象。`Array.from`都可以将它们转为真正的数组。

```javascript
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).forEach(function (p) {
  console.log(p);
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
```

只要是部署了 `Iterator` 接口的数据结构，`Array.from`都能将其转为数组。

```javascript
Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
```

对于还没有部署该方法的浏览器，可以用`Array.prototype.slice`方法替代。

`Array.from`还可以接受第二个参数，作用类似于数组的`map`方法。

```javascript
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
```

下面的例子是取出一组 DOM 节点的文本内容。

```javascript
let spans = document.querySelectorAll('span.name');

// map()
let names1 = Array.prototype.map.call(spans, s => s.textContent);

// Array.from()
let names2 = Array.from(spans, s => s.textContent)
```

下面的例子将数组中布尔值为`false`的成员转为0。

```javascript
Array.from([1, , 2, , 3], (n) => n || 0)
// [1, 0, 2, 0, 3]
```

如果`map`函数里面用到了`this`关键字，还可以传入`Array.from`的第三个参数，用来绑定`this`。

```javascript
Array.from({ length: 2 }, () => 'jack')
// ['jack', 'jack']
```

上面代码中，`Array.from`的第一个参数指定了第二个参数运行的次数。这种特性可以让该方法的用法变得非常灵活。

`Array.from()`的另一个应用是，将字符串转为数组，然后返回字符串的长度。

```javascript
function countSymbols(string) {
  return Array.from(string).length;
}
```

### Array.of()

`Array.of`方法用于将一组值，转换为数组。如果参数为空，则返回一个空数组。

```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
Array.of() // []
Array.of(undefined) // [undefined]
Array.of(1, 2) // [1, 2]
```

该方法的主要目的，是弥补数组构造函数`Array()`的不足。因为参数个数的不同，会导致`Array()`的行为有差异。

```javascript
Array() // [] 没有参数时，返回的时空数组
Array(3) // [, , ,]  只有一个参数时，返回的是数组的长度
Array(3, 11, 8) // [3, 11, 8]
```

兼容IE8:
```javascript
function ArrayOf(){
  return [].slice.call(arguments);
}
```

### copyWithin()

`copyWithin`方法，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。**会修改当前数组**。

```javascript
Array.prototype.copyWithin(target, start = 0, end = this.length)
```
    * target（必需）：从该位置开始替换数据。
    * start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。
    * end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

```javascript
[1, 2, 3, 4, 5].copyWithin(0, 3); // 同 [1, 2, 3, 4, 5].copyWithin(0, 3, 5)
// [4, 5, 3, 4, 5]
```

更多例子:
```javascript
// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}

// 将2号位到数组结束，复制到0号位
let i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]

// 对于没有部署 TypedArray 的 copyWithin 方法的平台
// 需要采用下面的写法
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]
```

### find(), findIndex()

数组实例的`find`方法，用于找出第一个符合条件的数组成员, 返回该成员。如果没有符合条件的成员，则返回`undefined`。

```javascript
[1, 4, -5, 10].find((n) => n < 0)
// -5

[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```

数组实例的`findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

```javascript
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

这两个方法都可以接受第二个参数，用来绑定回调函数的`this`对象。

这两个方法都可以发现`NaN`，弥补了数组的`indexOf`方法的不足。

```javascript
[NaN].indexOf(NaN)
// -1

[NaN].findIndex(y => Object.is(NaN, y))
// 0
```

上面代码中，`indexOf`方法无法识别数组的`NaN`成员，但是`findIndex`方法可以借助`Object.is`方法做到。

### fill()

`fill`方法使用给定值，填充一个数组。

```javascript
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
```

`fill`方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。**会改变原数组**

```javascript
['a', 'b', 'c'].fill(7, 1, 2) // 从 1 号位开始，向原数组填充 7，到 2 号位之前结束。
// ['a', 7, 'c']

let arr = [1, 2, 3, 4];
arr.fill(6, 1, 3); // [1, 6, 6, 4]
```

扩展：
```javascript
[].fill.call( { length : 3 }, 4) // {0: 4, 1: 4, 2: 4, length: 3}
```

### entries(), keys(), values()

`entries()`，`keys()`和`values()`——用于遍历数组。它们都返回一个遍历器对象，可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历(谷歌目前不支持)，`entries()`是对键值对的遍历。

```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

如果不使用`for...of`循环，可以手动调用遍历器对象的`next`方法，进行遍历。

```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
```

### includes()

`Array.prototype.includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的`includes`方法类似。

```javascript
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```

该方法的第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。

```javascript
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
```

没有该方法之前，我们通常使用数组的`indexOf`方法，检查是否包含某个值。

```javascript
if (arr.indexOf(el) !== -1) {
  // ...
}
```

`indexOf`方法内部使用严格相等运算符（`===`）进行判断，这会导致对`NaN`的误判。`includes`没有这个问题。

```javascript
[NaN].indexOf(NaN)
// -1

[NaN].includes(NaN)
// true
```

不支持includes时可用以下方法代替：
```javascript
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(['foo', 'bar'], 'baz'); // => false
```

### 其他

1. 数组求最大值：

```javascript
// ES5 的写法
Math.max.apply(null, [14, 3, 77])

// ES6 的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

2. 通过`push`函数，将一个数组添加到另一个数组的尾部。

```javascript
// ES5的 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);

// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```

上面代码的`ES5`写法中，`push`方法的参数不能是数组，所以只好通过`apply`方法变通使用`push`方法。有了扩展运算符，就可以直接将数组传入`push`方法。

3. 扩展运算符的应用

    * 复制数组
    数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

    ```javascript
    const a1 = [1, 2];
    const a2 = a1;

    a2[0] = 2;
    a1 // [2, 2]
    ```

    `ES5`只能用变通方法来复制数组。

    ```javascript
    const a1 = [1, 2];
    const a2 = a1.concat(); // a1会返回原数组的克隆，再修改a2就不会对a1产生影响。

    a2[0] = 2;
    a1 // [1, 2]
    ```

    扩展运算符提供了复制数组的简便写法。

    ```javascript
    const a1 = [1, 2];
    // 写法一
    const a2 = [...a1];
    // 写法二
    const [...a2] = a1;
    ```
    上面的两种写法，`a2`都是`a1`的克隆。

    * 合并数组

    ```javascript
    // ES5
    [1, 2].concat(more)
    // ES6
    [1, 2, ...more]

    var arr1 = ['a', 'b'];
    var arr2 = ['c'];
    var arr3 = ['d', 'e'];

    // ES5的合并数组
    arr1.concat(arr2, arr3);
    // [ 'a', 'b', 'c', 'd', 'e' ]

    // ES6的合并数组
    [...arr1, ...arr2, ...arr3]
    // [ 'a', 'b', 'c', 'd', 'e' ]
    ```
    * 与解构赋值结合

    扩展运算符可以与解构赋值结合起来，用于生成数组。

    ```javascript
    // ES5
    a = list[0], rest = list.slice(1)
    // ES6
    [a, ...rest] = list
    ```

    另外一些例子。

    ```javascript
    const [first, ...rest] = [1, 2, 3, 4, 5];
    first // 1
    rest  // [2, 3, 4, 5]

    const [first, ...rest] = [];
    first // undefined
    rest  // []

    const [first, ...rest] = ["foo"];
    first  // "foo"
    rest   // []
    ```

    如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

    ```javascript
    const [...butLast, last] = [1, 2, 3, 4, 5];
    // 报错

    const [first, ...middle, last] = [1, 2, 3, 4, 5];
    // 报错
    ```
4. 数组的空位

空位不是`undefined`，一个位置的值等于`undefined`，依然是有值的。空位是没有任何值，`in`运算符可以说明这一点。
```javascript
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```

ES5 对空位的处理，已经很不一致了，大多数情况下会忽略空位。

    * `forEach()`, `filter()`, `every()` 和`some()`都会跳过空位。
    * `map()`会跳过空位，但会保留这个值
    * `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

```javascript
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"
```

ES6 则是明确将空位转为`undefined`。

`Array.from`方法会将数组的空位，转为`undefined`，也就是说，这个方法不会忽略空位。

```javascript
Array.from(['a',,'b'])
// [ "a", undefined, "b" ]
```

扩展运算符（`...`）也会将空位转为`undefined`。
```javascript
[...['a',,'b']]
// [ "a", undefined, "b" ]
```

`copyWithin()`会连空位一起拷贝。
```javascript
[,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
```

`fill()`会将空位视为正常的数组位置。
```javascript
new Array(3).fill('a') // ["a","a","a"]
```

`for...of`循环也会遍历空位。
```javascript
let arr = [, ,];
for (let i of arr) {
  console.log(1);
}
// 1
// 1
```

上面代码中，数组arr有两个空位，`for...of`并没有忽略它们。如果改成`map`方法遍历，空位是会跳过的。

`entries()`、`keys()`、`values()`、`find()`和`findIndex()`会将空位处理成`undefined`。
```javascript
// entries()
[...[,'a'].entries()] // [[0,undefined], [1,"a"]]

// keys()
[...[,'a'].keys()] // [0,1]

// values()
[...[,'a'].values()] // [undefined,"a"]

// find()
[,'a'].find(x => true) // undefined

// findIndex()
[,'a'].findIndex(x => true) // 0
```

空位的处理规则非常不统一，所以建议避免出现空位。

参考文章：

  [Javascript标准参考教程 Array对象](http://javascript.ruanyifeng.com/stdlib/array.html)

  [ECMAScript6入门 数组的扩展](http://es6.ruanyifeng.com/#docs/array)
