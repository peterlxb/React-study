快速排序使用分治法（Divide and conquer）策略来把一个串行（list）分为两个子串行（sub-lists）。

快速排序又是一种分而治之思想在排序算法上的典型应用。本质上来看，快速排序应该算是在冒泡排序基础上的递归分治法。

## 算法步骤

> **  1.从数列中挑出一个元素，称为 “基准”（pivot）;**
>
> **  2.重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；**
>
> **  3.递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；**

递归的最底部情形，是数列的大小是零或一，也就是永远都已经被排序好了。虽然一直递归下去，但是这个算法总会退出，因为在每次的迭代（iteration）中，它至少会把一个元素摆到它最后的位置去。

![](https://raw.githubusercontent.com/hustcc/JS-Sorting-Algorithm/master/res/quickSort.gif)

```
quickSort = (arr) => {
  //交换数列中的两个元素
  const swap = (array, i , j) => {
    [array[i], array[j]] = [array[j], array[i]];
  }

  //分区
  const partition = (array, start, end) => {
    let splitIndex = start + 1;
    for (let i = start + 1; i <= end; i++) {
      if (array[i] < array[start]) {
        swap(array, i, splitIndex);
        splitIndex++;
      }
    }

    // 把 pivot 跟最后一个比它小的元素互换

    swap(array, start, splitIndex - 1);
    return splitIndex - 1;
  }

  //递归地把小于基准的子数列与大于基准的子数列重新排列
  const _quickSort = (array, start, end) => {
    if (start >= end) return array;

    // 在 partition 里面调整数列，并且回传 pivot 的 index

    const middle = partition(array, start, end);
    console.log("before sort: ",array)
    //先递归地排左边的子数列
    _quickSort(array, start, middle - 1);
    console.log("Left sort: ",array)
    //再递归地排右边的子数列
    _quickSort(array, middle + 1, end);
    console.log("Right sort: ",array)
    return array;
  };
  return _quickSort(arr, 0, arr.length - 1);
}
```

```
归并排序耗时: 0.112ms
start and end:  0 6
pivot:  14
before sort: [ 10, 7, 6, 9, 14, 20, 15 ]
start and end:  0 3
pivot:  10
before sort: [ 9, 7, 6, 10, 14, 20, 15 ]
start and end:  0 2
pivot:  9
before sort: [ 6, 7, 9, 10, 14, 20, 15 ]
start and end:  0 1
pivot:  6
before sort: [ 6, 7, 9, 10, 14, 20, 15 ]
start and end:  0 -1
Left sort: [ 6, 7, 9, 10, 14, 20, 15 ]
start and end:  1 1
Right sort:  [ 6, 7, 9, 10, 14, 20, 15 ]
Left sort: [ 6, 7, 9, 10, 14, 20, 15 ]
start and end:  3 2
Right sort:  [ 6, 7, 9, 10, 14, 20, 15 ]
Left sort: [ 6, 7, 9, 10, 14, 20, 15 ]
start and end:  4 3
Right sort:  [ 6, 7, 9, 10, 14, 20, 15 ]
Left sort: [ 6, 7, 9, 10, 14, 20, 15 ]
start and end:  5 6
pivot:  20
before sort: [ 6, 7, 9, 10, 14, 15, 20 ]
start and end:  5 5
Left sort: [ 6, 7, 9, 10, 14, 15, 20 ]
start and end:  7 6
Right sort:  [ 6, 7, 9, 10, 14, 15, 20 ]
Right sort:  [ 6, 7, 9, 10, 14, 15, 20 ]
[ 6, 7, 9, 10, 14, 15, 20 ]
```



