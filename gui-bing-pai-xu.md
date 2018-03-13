# Merge Sort归并排序

原理就是把两个已经各自排序好的序列合并成一个。

![](/assets/merge-sort.png)

实现代码

更简洁的写法

```
function mergeSort(array) {  //采用自上而下的递归方法
  var length = array.length;
  if(length < 2) {
    return array;
  }
  var m = (length >> 1),
      left = array.slice(0, m),
      right = array.slice(m); //拆分为两个子数组
  return merge(mergeSort(left), mergeSort(right));//子数组继续递归拆分,然后再合并
}
function merge(left, right){ //合并两个子数组
  var result = [];
  while (left.length && right.length) {
    var item = left[0] <= right[0] ? left.shift() : right.shift();//注意:判断的条件是小于或等于,如果只是小于,那么排序将不稳定.
    result.push(item);
  }
  return result.concat(left.length ? left : right);
}
```



```
function mergeSort(arr) {  //采用自上而下的递归方法
    console.log("Starting calling mergeSort ");
    var len = arr.length;
    if (len < 2) {
        console.log("arr in mergeSort ",arr);
        return arr;
    }
    var middle = Math.floor(len / 2),
        left = arr.slice(0, middle),
        right = arr.slice(middle);

    console.log("mergeSort left ",left);
    console.log("mergeSort right ", right);

    return merge(mergeSort(left), mergeSort(right));
    //mergeSort(left);
    //mergeSort(right);
}

function merge(left, right) {
    console.log("Starting calling merge ");
    console.log("Begin left ", left);
    console.log("Begin right ", right);

    var result = [];
    console.time('归并排序耗时');

    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift()); // left 删除第一个元素并返回
        } else {
            result.push(right.shift());
        }
    }

    console.log("After shift left ",left);
    console.log("After shift right ",right);

    while (left.length)
        result.push(left.shift());

    while (right.length)
        result.push(right.shift());

    console.log("result", result);
    console.timeEnd('归并排序耗时');
    return result;
}


var arr = [38,27,43,3,9,82,10,11,5];
console.log(mergeSort(arr));
```

打印出来的结果

```
Starting calling mergeSort 
mergeSort left  [ 38, 27, 43, 3 ]
mergeSort right  [ 9, 82, 10, 11, 5 ]  --------第一次分割整个序列
Starting calling mergeSort             ------以左边序列为一个整体继续分割
mergeSort left  [ 38, 27 ] ----再次调用mergeSort分割mergeSort([ 38, 27 ])
mergeSort right  [ 43, 3 ]
Starting calling mergeSort              --------再次分割
mergeSort left  [ 38 ]
mergeSort right  [ 27 ]
Starting calling mergeSort  ----- mergeSort([38])
arr in mergeSort  [ 38 ]
Starting calling mergeSort 
arr in mergeSort  [ 27 ]    ----- mergeSort([27])

Starting calling merge      --------实际调用merge([38], [27]);

Begin left  [ 38 ]
Begin right  [ 27 ]
After shift left  [ 38 ]
After shift right  []
result [ 27, 38 ]                      ----分割排序完
归并排序耗时: 0.436ms
Starting calling mergeSort 
mergeSort left  [ 43 ]
mergeSort right  [ 3 ]
Starting calling mergeSort 
arr in mergeSort  [ 43 ]
Starting calling mergeSort 
arr in mergeSort  [ 3 ]
Starting calling merge      -------实际调用merge([43], [3]);


Begin left  [ 43 ]
Begin right  [ 3 ]
After shift left  [ 43 ]
After shift right  []
result [ 3, 43 ]                  ----分割排序完
归并排序耗时: 0.145ms

Starting calling merge    调用merge([ 27, 38 ], [ 3, 43 ]);

Begin left  [ 27, 38 ]   
Begin right  [ 3, 43 ]
After shift left  []
After shift right  [ 43 ]
result [ 3, 27, 38, 43 ]          -----左边排序完
归并排序耗时: 0.193ms
Starting calling mergeSort        ------开始分割右边
mergeSort left  [ 9, 82 ]
mergeSort right  [ 10, 11, 5 ]
Starting calling mergeSort 
mergeSort left  [ 9 ]
mergeSort right  [ 82 ]
Starting calling mergeSort 
arr in mergeSort  [ 9 ]
Starting calling mergeSort 
arr in mergeSort  [ 82 ]
Starting calling merge 
Begin left  [ 9 ]
Begin right  [ 82 ]
After shift left  []
After shift right  [ 82 ]
result [ 9, 82 ]               ---分割排序完
归并排序耗时: 0.176ms
Starting calling mergeSort 
mergeSort left  [ 10 ]
mergeSort right  [ 11, 5 ]
Starting calling mergeSort 
arr in mergeSort  [ 10 ]
Starting calling mergeSort 
mergeSort left  [ 11 ]
mergeSort right  [ 5 ]
Starting calling mergeSort 
arr in mergeSort  [ 11 ]
Starting calling mergeSort 
arr in mergeSort  [ 5 ]
Starting calling merge 
Begin left  [ 11 ]
Begin right  [ 5 ]
After shift left  [ 11 ]
After shift right  []
result [ 5, 11 ]           -----小组分割排序完
归并排序耗时: 0.143ms
Starting calling merge 
Begin left  [ 10 ]
Begin right  [ 5, 11 ]
After shift left  []
After shift right  [ 11 ]
result [ 5, 10, 11 ]      -----小组分割排序完

归并排序耗时: 0.144ms
Starting calling merge 
Begin left  [ 9, 82 ]
Begin right  [ 5, 10, 11 ]
After shift left  [ 82 ]
After shift right  []
result [ 5, 9, 10, 11, 82 ]   -----右边分割排序完

归并排序耗时: 0.151ms
Starting calling merge    调用 merge([ 3, 27, 38, 43 ], [ 5, 9, 10, 11, 82 ]);



Begin left  [ 3, 27, 38, 43 ]
Begin right  [ 5, 9, 10, 11, 82 ]
After shift left  []
After shift right  [ 82 ]
result [ 3, 5, 9, 10, 11, 27, 38, 43, 82 ]
归并排序耗时: 0.207ms
[ 3, 5, 9, 10, 11, 27, 38, 43, 82 ]
```

本质上是一个嵌套很深的函数

```
merge(
    merge(merge(left),merge(right)),merge((merge(left1),merge(right1))
    )
```

> ```
> 归并排序原理
> 将序列分为子序列，最终分成两个元素进行比较或只有一个元素
>
> 代码实现也很简单，
> mergeSort 分割这个序列 分割到最小子序列时就不再分割，这个时候由merge开始处理
> 当这个arr个数小于2（1个或0）时直接返回不处理，
> 最后 mergeSort里传递给merge的两个参数left为一个值，right也为一个值，
> 这两个值在merge 的 while (left.length && right.length) 里处理，shift处理掉一个后就退出了这个循环
> 后面紧接着选择其中一个while循环，得到一个result。
> 步骤就是先将整个数组分成两部分，[38,27,43,3] 和 [9,82,10,11,5]
>
> 这其实是一个回调，嵌套很深，第一层 return merge(mergeSort(left), mergeSort(right)) ==> 
> mergeSort(left) ==> mergeSort([38,27,43,3]) ==> return merge(mergeSort([38,27]), mergeSort([43,3]))
> mergeSort([38,27]) ==> return merge(mergeSort([38]), mergeSort([27]))
>
> 这是左边的执行顺序，在 while (left.length && right.length)很容易处理，返回的结果result [27，38]
> mergeSort([43,3]) 就返回[3,43] 然后回到 merge([27，38]), [3,43])很容易通过while 循环排序 最后返回 [ 3, 27, 38, 43 ]
>
> 然后处理 第一层 return merge(mergeSort(left), mergeSort(right))中的 mergeSort(right)也就是mergeSort([9,82,10,11,5])
> 遵循一样的规则最后返回 [ 5, 9, 10, 11, 82 ]
>
> 然后就是 merge(left, right) ==> merge([ 3, 27, 38, 43 ], [ 5, 9, 10, 11, 82 ])
> ```



