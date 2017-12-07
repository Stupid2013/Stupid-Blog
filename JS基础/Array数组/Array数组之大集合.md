## Array数组方法大集合

2017-12-06

在我们的实际开发中，数组可谓是用到的最常见的数据结构了，随着js的发展，它的用法也越来越“高大上”，越来越强大。作为一名小菜鸟，也只对它一些常用的方法比较熟悉，有的时候甚至常用的都不熟悉。。。啊，说来惭愧。因此一直想要总结之，那么，今天！就是这一伟大时刻！！😎

集体照：

    valueOf(), toString(), push(), pop(), join(), concat(), shift(), unshift(), reverse(), slice(), splice(), sort()
    es5新增: forEach(), map(), filter(), some(), every(), indexOf(), lastIndexOf(), reduce(), reduceRight()
    es6新增： from(), of(), copyWithin(), find(), findIndex(), fill(), entries(), keys(), values(), includes()
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


参考文章：
  [Javascript标准参考教程 Array对象](http://javascript.ruanyifeng.com/stdlib/array.html)

  [ECMAScript 6入门 数组的扩展](http://es6.ruanyifeng.com/#docs/array)
