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
    //先递归地排左边的子数列
    _quickSort(array, start, middle - 1);
    //再递归地排右边的子数列
    _quickSort(array, middle + 1, end);
    return array;
  };
  return _quickSort(arr, 0, arr.length - 1);
}
```



