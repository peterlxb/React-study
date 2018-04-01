# 插入排序

![](https://blog.techbridge.cc/img/huli/sorting/insertion.gif)

插入排序原理就是先把第一个元素作为已经排序的。然后取出第二个与第一个进行排序，这两个排序完再取出第三个元素

与前两个已经排序完的进行比较。一次类推。每次取出一个元素排序与已经排序过的进行二次排序。

```
//插入排序
//类似打扑克牌，不断往前插到合适的位置
const  insertionSort = (arr) => {
    console.time('插入排序耗时');
    const n  = arr.length;
    //假设第一个arr[0]已经排序
    for (let i = 1; i < n; i++) {
        console.log("Starting from i ",i);
        //设置排序初始位置
        let position = i;
        
        //进行比较的元素
        const value = arr[i];

        while (i >=0 && arr[position - 1] > value ) {
            console.log("Inside while loop ", position);
            [arr[position],arr[position - 1]] = [arr[position - 1],arr[position]];
            //降序进行比较
            position--;
        }

        console.log("return inserted arr ",arr);
    }
    console.timeEnd('插入排序耗时');
    return arr;
}

var arr1 = [3, 44, 38, 5, 47, 15, 36, 26, 27, 2, 46, 4,];
console.log(insertionSort(arr1));
```

上面排序的问题是position--降序的算法。如果小的值在后面要经过很多次排序才能排到正确位置。如示例中的值2。

打印出来的过程如下

```
Starting from i  1
return inserted arr  [ 3, 44, 38, 5, 47, 15, 36, 26, 27, 2, 46, 4 ]
Starting from i  2
Inside while loop  2
return inserted arr  [ 3, 38, 44, 5, 47, 15, 36, 26, 27, 2, 46, 4 ]
Starting from i  3
Inside while loop  3
Inside while loop  2
return inserted arr  [ 3, 5, 38, 44, 47, 15, 36, 26, 27, 2, 46, 4 ]
Starting from i  4
return inserted arr  [ 3, 5, 38, 44, 47, 15, 36, 26, 27, 2, 46, 4 ]
Starting from i  5
Inside while loop  5
Inside while loop  4
Inside while loop  3
return inserted arr  [ 3, 5, 15, 38, 44, 47, 36, 26, 27, 2, 46, 4 ]
Starting from i  6
Inside while loop  6
Inside while loop  5
Inside while loop  4
return inserted arr  [ 3, 5, 15, 36, 38, 44, 47, 26, 27, 2, 46, 4 ]
Starting from i  7
Inside while loop  7
Inside while loop  6
Inside while loop  5
Inside while loop  4
return inserted arr  [ 3, 5, 15, 26, 36, 38, 44, 47, 27, 2, 46, 4 ]
Starting from i  8
Inside while loop  8
Inside while loop  7
Inside while loop  6
Inside while loop  5
return inserted arr  [ 3, 5, 15, 26, 27, 36, 38, 44, 47, 2, 46, 4 ]
Starting from i  9
Inside while loop  9
Inside while loop  8
Inside while loop  7
Inside while loop  6
Inside while loop  5
Inside while loop  4
Inside while loop  3
Inside while loop  2
Inside while loop  1
return inserted arr  [ 2, 3, 5, 15, 26, 27, 36, 38, 44, 47, 46, 4 ]
Starting from i  10
Inside while loop  10
return inserted arr  [ 2, 3, 5, 15, 26, 27, 36, 38, 44, 46, 47, 4 ]
Starting from i  11
Inside while loop  11
Inside while loop  10
Inside while loop  9
Inside while loop  8
Inside while loop  7
Inside while loop  6
Inside while loop  5
Inside while loop  4
Inside while loop  3
return inserted arr  [ 2, 3, 4, 5, 15, 26, 27, 36, 38, 44, 46, 47 ]
插入排序耗时: 15.569ms
[ 2, 3, 4, 5, 15, 26, 27, 36, 38, 44, 46, 47 ]
```

通过打印出来的结果可以很容易理解插入排序的原理，第一个元素也就是arr\[0\]作为第一个参考，循环时从i=1开始。

i = 1 时 直接跳过不执行while循环。

以此类推。

1 = 2 38和44呼唤位置。

