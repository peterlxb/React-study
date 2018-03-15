# Array.prototype.reduce补充
Redux reducer名字其实是由Array的reduce方法中借用过来的。
reduce的功能就是接收一个集合的数据然后通过处理返回一个单一的数据。
语法很简单
>arr.reduce(callback[, initialValue])

它里面的回调函数接收四个参数，
1. accumulator累加器
2. currentValue 目前的值
3. currentIndex 目前的索引
4. array 待处理数组

这个回调函数第一次被调用时 accumulator累加器 和 currentValue取值有两种情况。
如果这个reduce提供了initialValue，那么第一次调用时的accumulator累加器就是initialValue初始值。currentValue就是array的第一个值。
如果reduce没有提供initialValue,那么accumulator累加器就是第一个值，currentValue就是array的第二个值。
最好提供initialValue。

reduce工作的原理。
以这个示例开始
```JS
[0, 1, 2, 3, 4].reduce(
  function (
    accumulator,
    currentValue,
    currentIndex,
    array
  ) {
    return accumulator + currentValue;
  }
);

```
这里面的回调函数会被调用四次。每次累加的结果汇总到最后。

示例1
```JS
const iceCreamStats = [
  {name:'Amanda',gallonsEaten:3.8},
  {name:'Geoff',gallonsEaten:5.2},
  {name:'Tyler',gallonsEaten:1.9},
  {name:'Richard',gallonsEaten:7923}
];

//call reduce
iceCreamStats.reduce((accumulator,currentValue) => {
  return accumulator + currentValue.gallonsEaten;
},0);

//return 7933.9


```

示例2 展平一个数组。initialValue设置为一个空数组。
```JS
const flattened = [[0,1], [2,3], [4,5]].reduce(
  (accumulator, currentValue) => accumulator.concat(currentValue),
  []
);
```

示例3 统计某个值出现的次数。返回一个对象。
```JS
const name = ["Alice", "Bob", "Tiff", "Bruce", "Alice"];

const countedNames = name.reduce((allNames,currentName) => {
  if (currentName in allNames) {
    allNames[currentName]++
  } else {
    allNames[currentName] = 1;
  }
  return allNames
}, {});

// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```

示例4 数组中时对象，对象中有数组的操作。使用了spread operator和initialValue.
```JS
var friends = [{
  name: 'Anna',
  books: ['Bible', 'Harry Potter'],
  age: 21
}, {
  name: 'Bob',
  books: ['War and peace', 'Romeo and Juliet'],
  age: 26
}, {
  name: 'Alice',
  books: ['The Lord of the Rings', 'The Shining'],
  age: 18
}];

// allbooks - list which will contain all friends' books +  
// additional list contained in initialValue
var allbooks = friends.reduce(function(accumulator, currentValue) {
  console.log(accumulator,currentValue.books);
  return [...accumulator, ...currentValue.books];
}, ['Alphabet']);

// allbooks = [
//   'Alphabet', 'Bible', 'Harry Potter', 'War and peace',
//   'Romeo and Juliet', 'The Lord of the Rings',
//   'The Shining'
// ]

在console总可以清楚看到accumulator和currentValue值的变化。
// ["Alphabet"] (2) ["Bible", "Harry Potter"]
// ["Alphabet", "Bible", "Harry Potter"] (2) ["War and peace", "Romeo and Juliet"]
// ["Alphabet", "Bible", "Harry Potter", "War and peace", "Romeo and Juliet"] (2) ["The Lord of the Rings", "The Shining"]

```

示例5 排除重复的数字
```JS
let arr = [1, 2, 1, 2, 3, 5, 4, 5, 3, 4, 4, 4, 4];
let result = arr.sort().reduce((accumulator, current) => {
    const length = accumulator.length
    if (length === 0 || accumulator[length - 1] !== current) { //第一个值肯定要push
        accumulator.push(current);
    }
    return accumulator;
}, []);
console.log(result); //[1,2,3,4,5]
```
