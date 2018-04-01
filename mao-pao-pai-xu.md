# Bubble Sort冒泡排序

原理很简单，与隔壁互相比较，顺序错了就交换，让大的元素一只浮到最后。

用一张图来表示

![](/assets/bubble.png)

\(图片来源：

[http://www.opentechguides.com/how-to/article/c/51/bubble-sort-c.html](http://www.opentechguides.com/how-to/article/c/51/bubble-sort-c.html)

）

用ES6的语法实现最简单版本的冒泡排序

```
const bubbleSort = (arr) => {
  const n = arr.length;

  // 一共要跑 n遍

  for (let i = 0; i < n; i++) {
    console.log("Begin in i loop",i);
    // 从第一个元素开始，不断跑到第 n - 1 - i 个

    // 原本是 n - 1，会再加上 - i 是因为最后 i 个元素已经排好了，最后一个是最大的，不用排序

    // 沒必要跟那些排好的元素比较

    for (let j = 0; j < n - 1 - i; j++) {
      console.log("Inside J ", j);
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
      console.log(arr)
    }
  }

  return arr;
}
//打印结果
console.log(bubbleSort([5, 6, 31, 23, 9, 14, 11, 2]));
```

console中打印的结果

```
Begin in i loop 0              ------第一次 相邻两个元素比较，第一次比较找出最大元素
Inside J  0
[ 5, 6, 31, 23, 9, 14, 11, 2 ]
Inside J  1
[ 5, 6, 31, 23, 9, 14, 11, 2 ]
Inside J  2
[ 5, 6, 23, 31, 9, 14, 11, 2 ]
Inside J  3
[ 5, 6, 23, 9, 31, 14, 11, 2 ]
Inside J  4
[ 5, 6, 23, 9, 14, 31, 11, 2 ]
Inside J  5
[ 5, 6, 23, 9, 14, 11, 31, 2 ]
Inside J  6
[ 5, 6, 23, 9, 14, 11, 2, 31 ]     // 第一次比较完找出最大的31
Begin in i loop 1            ------------第二次
Inside J  0
[ 5, 6, 23, 9, 14, 11, 2, 31 ]
Inside J  1
[ 5, 6, 23, 9, 14, 11, 2, 31 ]
Inside J  2
[ 5, 6, 9, 23, 14, 11, 2, 31 ]
Inside J  3
[ 5, 6, 9, 14, 23, 11, 2, 31 ]
Inside J  4
[ 5, 6, 9, 14, 11, 23, 2, 31 ]
Inside J  5
[ 5, 6, 9, 14, 11, 2, 23, 31 ]   // 同样的原理，找出第二大的23
Begin in i loop 2            ------------第三次
Inside J  0
[ 5, 6, 9, 14, 11, 2, 23, 31 ]
Inside J  1
[ 5, 6, 9, 14, 11, 2, 23, 31 ]
Inside J  2
[ 5, 6, 9, 14, 11, 2, 23, 31 ]
Inside J  3
[ 5, 6, 9, 11, 14, 2, 23, 31 ]
Inside J  4
[ 5, 6, 9, 11, 2, 14, 23, 31 ] // 找到14
Begin in i loop 3           --------------第四次
Inside J  0
[ 5, 6, 9, 11, 2, 14, 23, 31 ]
Inside J  1
[ 5, 6, 9, 11, 2, 14, 23, 31 ]
Inside J  2
[ 5, 6, 9, 11, 2, 14, 23, 31 ]
Inside J  3
[ 5, 6, 9, 2, 11, 14, 23, 31 ]
Begin in i loop 4          ----------------第五次
Inside J  0
[ 5, 6, 9, 2, 11, 14, 23, 31 ]
Inside J  1
[ 5, 6, 9, 2, 11, 14, 23, 31 ]
Inside J  2
[ 5, 6, 2, 9, 11, 14, 23, 31 ]
Begin in i loop 5         -----------------第六次
Inside J  0
[ 5, 6, 2, 9, 11, 14, 23, 31 ]
Inside J  1
[ 5, 2, 6, 9, 11, 14, 23, 31 ]
Begin in i loop 6         -------------第七次
Inside J  0
[ 2, 5, 6, 9, 11, 14, 23, 31 ] // 在这里其实不用再继续排序
Begin in i loop 7         -------------- 第八次
[ 2, 5, 6, 9, 11, 14, 23, 31 ]
```

## 优化冒泡排序

可以加上一个flag来标注内圈有没有交换的发生。如果没有就代表已经是排序好的，可以直接跳过。

> 设置一标志性变量pos,用于记录每趟排序中最后一次进行交换的位置。由于pos位置之后的记录均已交换到位,故在进行下一趟排序时只要扫描到pos位置即可。

改进后算法如下:

```
const bubbleSort2 = (arr) => {
    console.time('改进后冒泡排序耗时');
    let i = arr.length-1;  //初始时,最后位置保持不变
    while ( i> 0) {
        let pos= 0; //每趟开始时,无记录交换 所以开始时pos清零
        for (let j= 0; j< i; j++)
            if (arr[j]> arr[j+1]) {
                pos= j; //记录交换的位置
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        i= pos; //为下一趟排序作准备 最后pos为0，循环结束。
     }
     console.timeEnd('改进后冒泡排序耗时');
     return arr;
}

```

改进版:加了个swapCount用来计数，如果为0说明没有进行交换，就可以直接退出循环打印出数组。

```
const bubbleSort = (arr) => {
    const n = arr.length;
    //用来进行计数，如果发生交换就加1
    let swapCount;
    
    for (let i = 0; i < n; i++) {
      swapCount = 0;
     
      for (let j = 0; j < n - 1 - i; j++) {
        
        if (arr[j] > arr[j + 1]) {
          [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
          swapCount++;
        }
      }
      //没有进行交换说明已经排序好
      if(swapCount == 0){
          break;
      }
    }
    return arr;
  }

console.log(bubbleSort([5, 6, 7, 8, 9, 11, 14]));

```

上面的改进版在排序那些已经排好序的数组时只需要跑一次就行。

第一版无论是否排序好都要跑要跑n\*n次。

