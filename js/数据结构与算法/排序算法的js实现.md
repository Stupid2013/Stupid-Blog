### 排序算法的js实现

2018-04-02

1、冒泡排序

**原始的冒泡排序**

```javascript
const order = [6, 3, 8, 2, 9, 1];
console.time('冒泡排序耗时');
for(let i = 0; i < order.length - 1; i++){ //外层循环控制排序趟数
  for(let j = 0; j < order.length - 1 - i; j++){ //内层循环控制每一趟排序多少次
    if(order[j] > order[j+1]){
      let temp = order[j];
      order[j] = order[j+1];
      order[j+1] = temp;
    }
    // console.log('==== i ', i);
    // console.log('==== j ', j);
    // console.log('=== order ', order);
  }
  // console.log('-----------------------------');
}
console.timeEnd('冒泡排序耗时');
```
原理：相邻的两个元素进行比较，将值大的元素交换至右边。

实现：

第一趟排序：

　　　　第一次排序：6和3比较，6大于3，交换位置：  3  6  8  2  9  1

　　　　第二次排序：6和8比较，6小于8，不交换位置：3  6  8  2  9  1

　　　　第三次排序：8和2比较，8大于2，交换位置：  3  6  2  8  9  1

　　　　第四次排序：8和9比较，8小于9，不交换位置：3  6  2  8  9  1

　　　　第五次排序：9和1比较：9大于1，交换位置：  3  6  2  8  1  9

　　　　第一趟总共进行了5次比较， 排序结果：      3  6  2  8  1  9

---

第二趟排序：

　　　　第一次排序：3和6比较，3小于6，不交换位置：3  6  2  8  1  9

　　　　第二次排序：6和2比较，6大于2，交换位置：  3  2  6  8  1  9

　　　　第三次排序：6和8比较，6大于8，不交换位置：3  2  6  8  1  9

　　　　第四次排序：8和1比较，8大于1，交换位置：  3  2  6  1  8  9

　　　　第二趟总共进行了4次比较， 排序结果：      3  2  6  1  8  9

---

第三趟排序：

　　　　第一次排序：3和2比较，3大于2，交换位置：  2  3  6  1  8  9

　　　　第二次排序：3和6比较，3小于6，不交换位置：2  3  6  1  8  9

　　　　第三次排序：6和1比较，6大于1，交换位置：  2  3  1  6  8  9

　　　　第二趟总共进行了3次比较， 排序结果：     2  3  1  6  8  9

---

第四趟排序：

　　　　第一次排序：2和3比较，2小于3，不交换位置：2  3  1  6  8  9

　　　　第二次排序：3和1比较，3大于1，交换位置：  2  1  3  6  8  9

　　　　第二趟总共进行了2次比较， 排序结果：     2  1  3  6  8  9

---

第五趟排序：

　　　　第一次排序：2和1比较，2大于1，交换位置：  1  2  3  6  8  9

　　　　第二趟总共进行了1次比较， 排序结果：      1  2  3  6  8  9

---

最终结果：1  2  3  6  8  9

---

由此可见：`n`个数字要排序完成，总共进行`n-1`趟排序，每i趟的排序次数为`(n-i)`次.

冒泡排序的优点：每进行一趟排序，就会少比较一次，因为每进行一趟排序都会找出一个较大值。

**进化版**

设置一标志性变量`pos`,用于记录每趟排序中最后一次进行交换的位置。由于`pos`位置之后的记录均已交换到位,故在进行下一趟排序时只要扫描到`pos`位置即可。

```javascript
const order = [6, 3, 8, 2, 9, 1];
console.time('改进后冒泡排序耗时');
var i = order.length-1;  //初始时,最后位置保持不变
while ( i > 0) {
    var pos= 0; //每趟开始时,无记录交换
    for (var j= 0; j< i; j++) {
      if (order[j]> order[j+1]) {
        pos= j; //记录交换的位置
        var tmp = order[j]; order[j]=order[j+1];order[j+1]=tmp;
      }
    }
    i = pos; //为下一趟排序作准备
 }
 console.timeEnd('改进后冒泡排序耗时');
```

**升级版**

传统冒泡排序中每一趟排序操作只能找到一个最大值或最小值,我们考虑利用在每趟排序中进行正向和反向两遍冒泡的方法一次可以得到两个最终值(最大者和最小者) , 从而使排序趟数几乎减少了一半。

```javascript
var low = 0;
var high= order.length-1; //设置变量的初始值
var tmp,j;
console.time('升级版冒泡排序耗时');
while (low < high) {
    for (j= low; j< high; ++j) { //正向冒泡,找到最大者
      if (order[j]> order[j+1]) {
          tmp = order[j]; order[j]=order[j+1];order[j+1]=tmp;
      }
    }
    --high;                 //修改high值, 前移一位
    for (j=high; j>low; --j) { //反向冒泡,找到最小者
      if (order[j]<order[j-1]) {
          tmp = order[j]; order[j]=order[j-1];order[j-1]=tmp;
      }
    }
    ++low;                  //修改low值,后移一位
}
console.timeEnd('升级版冒泡排序耗时');
```

数据量小的时候， 其实可以看出来三种方法的区别是不大的，理论上应该越来越快的（毕竟比较次数越来越少），数据多的时候应该比较明显 = = 无聊的时候可以一试 = =

实际上，我的结论是：数据量小的时候其实速度还是相反的！！！（比如上面的demo里的数据，执行时间依次大概是：0.021，0.032，0.033）大概是因为后面两种方法定义和判断多一些吧！

2、选择排序

工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

```javascript
var len = order.length;
var minIndex, temp;
console.time('选择排序耗时');
for (var i = 0; i < len - 1; i++) {
    minIndex = i;
    for (var j = i + 1; j < len; j++) {
        if (order[j] < order[minIndex]) {     //寻找最小的数
            minIndex = j;                 //将最小数的索引保存
        }
    }
    temp = order[i];
    order[i] = order[minIndex];
    order[minIndex] = temp;
}
console.timeEnd('选择排序耗时');
```

3、插入排序

它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。插入排序在实现上，通常采用in-place排序（即只需用到O(1)的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。

```javascript
console.time('插入排序耗时：');
for (var i = 1; i < order.length; i++) {
    var key = order[i];
    var j = i - 1;
    while (j >= 0 && order[j] > key) {
        order[j + 1] = order[j];
        j--;
    }
    order[j + 1] = key;
}
console.timeEnd('插入排序耗时：');
```

改进插入排序： 查找插入位置时使用二分查找的方式

```javascript
console.time('二分插入排序耗时：');
for (var i = 1; i < order.length; i++) {
    var key = order[i], left = 0, right = i - 1;
    while (left <= right) {
        var middle = parseInt((left + right) / 2);
        if (key < order[middle]) {
            right = middle - 1;
        } else {
            left = middle + 1;
        }
    }
    for (var j = i - 1; j >= left; j--) {
        order[j + 1] = order[j];
    }
    order[left] = key;
}
console.timeEnd('二分插入排序耗时：');
```

4、希尔排序

是简单插入排序的改进版；它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫缩小增量排序。

```javascript
var len = order.length,
  temp,
  gap = 1;
console.time('希尔排序耗时:');
while(gap < len/5) {          //动态定义间隔序列
    gap =gap*5+1;
}
for (gap; gap > 0; gap = Math.floor(gap/5)) {
    for (var i = gap; i < len; i++) {
        temp = order[i];
        for (var j = i-gap; j >= 0 && order[j] > temp; j-=gap) {
            order[j+gap] = order[j];
        }
        order[j+gap] = temp;
    }
}
console.timeEnd('希尔排序耗时:');
```

5、归并排序

将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。

```javascript
function mergeSort(arr) {  //采用自上而下的递归方法
    var len = arr.length;
    if(len < 2) {
        return arr;
    }
    var middle = Math.floor(len / 2),
        left = arr.slice(0, middle),
        right = arr.slice(middle);
    return merge(mergeSort(left), mergeSort(right));
}
function merge(left, right)
{
    var result = [];
    console.time('归并排序耗时');
    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }
    while (left.length)
        result.push(left.shift());
    while (right.length)
        result.push(right.shift());
    console.timeEnd('归并排序耗时');
    return result;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(mergeSort(arr));
```

6、快速排序

快速排序的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

```javascript
/*方法说明：快速排序
@param  array 待排序数组*/
//方法一
function quickSort(array, left, right) {
  console.time('1.快速排序耗时');
  if (Object.prototype.toString.call(array).slice(8, -1) === 'Array' && typeof left === 'number' && typeof right === 'number') {
      if (left < right) {
        var x = array[right], i = left - 1, temp;
        for (var j = left; j <= right; j++) {
          if (array[j] <= x) {
            i++;
            temp = array[i];
            array[i] = array[j];
            array[j] = temp;
          }
        }
        quickSort(array, left, i - 1);
        quickSort(array, i + 1, right);
      }
      console.timeEnd('1.快速排序耗时');
      return array;
  } else {
      return 'array is not an Array or left or right is not a number!';
  }
}
//方法二
var quickSort2 = function(arr) {
    console.time('2.快速排序耗时');
　　if (arr.length <= 1) { return arr; }
　　var pivotIndex = Math.floor(arr.length / 2);
　　var pivot = arr.splice(pivotIndex, 1)[0];
　　var left = [];
　　var right = [];
　　for (var i = 0; i < arr.length; i++){
　　　　if (arr[i] < pivot) {
　　　　　　left.push(arr[i]);
　　　　} else {
　　　　　　right.push(arr[i]);
　　　　}
　　}
console.timeEnd('2.快速排序耗时');
　　return quickSort2(left).concat([pivot], quickSort2(right));
};
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(quickSort(arr,0,arr.length-1));//[2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]
console.log(quickSort2(arr));//[2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]
```

7、堆排序

```javascript
/*方法说明：堆排序
@param  array 待排序数组*/
function heapSort(array) {
    console.time('堆排序耗时');
    if (Object.prototype.toString.call(array).slice(8, -1) === 'Array') {
        //建堆
        var heapSize = array.length, temp;
        for (var i = Math.floor(heapSize / 2) - 1; i >= 0; i--) {
            heapify(array, i, heapSize);
        }
        //堆排序
        for (var j = heapSize - 1; j >= 1; j--) {
            temp = array[0];
            array[0] = array[j];
            array[j] = temp;
            heapify(array, 0, --heapSize);
        }
        console.timeEnd('堆排序耗时');
        return array;
    } else {
        return 'array is not an Array!';
    }
}
/*方法说明：维护堆的性质
@param  arr 数组
@param  x   数组下标
@param  len 堆大小*/
function heapify(arr, x, len) {
  if (Object.prototype.toString.call(arr).slice(8, -1) === 'Array' && typeof x === 'number') {
    var l = 2 * x + 1, r = 2 * x + 2, largest = x, temp;
    if (l < len && arr[l] > arr[largest]) {
        largest = l;
    }
    if (r < len && arr[r] > arr[largest]) {
        largest = r;
    }
    if (largest != x) {
        temp = arr[x];
        arr[x] = arr[largest];
        arr[largest] = temp;
        heapify(arr, largest, len);
    }
  } else {
    return 'arr is not an Array or x is not a number!';
  }
}
var arr=[91,60,96,13,35,65,46,65,10,30,20,31,77,81,22];
console.log(heapSort(arr));//[10, 13, 20, 22, 30, 31, 35, 46, 60, 65, 65, 77, 81, 91, 96]
```

8、计数排序

计数排序使用一个额外的数组C，其中第i个元素是待排序数组A中值等于i的元素的个数。然后根据数组C来将A中的元素排到正确的位置。它只能对整数进行排序。

```javascript
function countingSort(array) {
  var len = array.length,
      B = [],
      C = [],
      min = max = array[0];
  console.time('计数排序耗时');
  for (var i = 0; i < len; i++) {
      min = min <= array[i] ? min : array[i];
      max = max >= array[i] ? max : array[i];
      C[array[i]] = C[array[i]] ? C[array[i]] + 1 : 1;
  }
  for (var j = min; j < max; j++) {
      C[j + 1] = (C[j + 1] || 0) + (C[j] || 0);
  }
  for (var k = len - 1; k >= 0; k--) {
      B[C[array[k]] - 1] = array[k];
      C[array[k]]--;
  }
  console.timeEnd('计数排序耗时');
  return B;
}
var arr = [2, 2, 3, 8, 7, 1, 2, 2, 2, 7, 3, 9, 8, 2, 1, 4, 2, 4, 6, 9, 2];
console.log(countingSort(arr)); //[1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 4, 4, 6, 7, 7, 8, 8, 9, 9]
```

9、桶排序

桶排序是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。

桶排序 (Bucket sort)的工作的原理：假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排

```javascript
/*方法说明：桶排序
@param  array 数组
@param  num   桶的数量*/
function bucketSort(array, num) {
    if (array.length <= 1) {
        return array;
    }
    var len = array.length, buckets = [], result = [], min = max = array[0], regex = '/^[1-9]+[0-9]*$/', space, n = 0;
    num = num || ((num > 1 && regex.test(num)) ? num : 10);
    console.time('桶排序耗时');
    for (var i = 1; i < len; i++) {
        min = min <= array[i] ? min : array[i];
        max = max >= array[i] ? max : array[i];
    }
    space = (max - min + 1) / num;
    for (var j = 0; j < len; j++) {
        var index = Math.floor((array[j] - min) / space);
        if (buckets[index]) {   //  非空桶，插入排序
            var k = buckets[index].length - 1;
            while (k >= 0 && buckets[index][k] > array[j]) {
                buckets[index][k + 1] = buckets[index][k];
                k--;
            }
            buckets[index][k + 1] = array[j];
        } else {    //空桶，初始化
            buckets[index] = [];
            buckets[index].push(array[j]);
        }
    }
    while (n < num) {
        result = result.concat(buckets[n]);
        n++;
    }
    console.timeEnd('桶排序耗时');
    return result;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(bucketSort(arr,4));//[2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]
```

10、基数排序

基数排序是按照低位先排序，然后收集；再按照高位排序，然后再收集；依次类推，直到最高位。有时候有些属性是有优先级顺序的，先按低优先级排序，再按高优先级排序。最后的次序就是高优先级高的在前，高优先级相同的低优先级高的在前。基数排序基于分别排序，分别收集，所以是稳定的。

```javascript
/**
 * 基数排序适用于：
 *  (1)数据范围较小，建议在小于1000
 *  (2)每个数值都要大于等于0
 * @author damonare
 * @param  arr 待排序数组
 * @param  maxDigit 最大位数
 */
//LSD Radix Sort
function radixSort(arr, maxDigit) {
    var mod = 10;
    var dev = 1;
    var counter = [];
    console.time('基数排序耗时');
    for (var i = 0; i < maxDigit; i++, dev *= 10, mod *= 10) {
        for(var j = 0; j < arr.length; j++) {
            var bucket = parseInt((arr[j] % mod) / dev);
            if(counter[bucket]== null) {
                counter[bucket] = [];
            }
            counter[bucket].push(arr[j]);
        }
        var pos = 0;
        for(var j = 0; j < counter.length; j++) {
            var value = null;
            if(counter[j]!=null) {
                while ((value = counter[j].shift()) != null) {
                      arr[pos++] = value;
                }
          }
        }
    }
    console.timeEnd('基数排序耗时');
    return arr;
}
var arr = [3, 44, 38, 5, 47, 15, 36, 26, 27, 2, 46, 4, 19, 50, 48];
console.log(radixSort(arr,2)); //[2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]

```


参考文章：

  [Java中的经典算法之冒泡排序(Bubble Sort)](https://www.cnblogs.com/shen-hua/p/5422676.html)

  [十大经典排序算法](http://blog.damonare.cn/2016/12/20/%E5%8D%81%E5%A4%A7%E7%BB%8F%E5%85%B8%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93%EF%BC%88javascript%E6%8F%8F%E8%BF%B0%EF%BC%89/)
