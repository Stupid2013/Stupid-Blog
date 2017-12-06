## Array数组方法大集合

在我们的实际开发中，数组可谓是用到的最常见的数据结构了，随着js的发展，它的用法也越来越“高大上”，越来越强大。作为一名小菜鸟，也只对它一些常用的方法比较熟悉，有的时候甚至常用的都不熟悉。。。啊，说来惭愧。因此一直想要总结之，那么，今天！就是这一伟大时刻！！😎

集体照：

    valueOf(), toString(), push(), pop(), join(), concat(), shift(), unshift(), splice(), reverse(), sort(), slice()
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

`concat`方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后面，然后返回一个新数组，**原数组不变**。

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
