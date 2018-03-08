# 

# Selection Sort 选择排序

原理就是找到最小值，移到最左边

```
const selectionSort = (arr) => {
    const length = arr.length;

    //有几个元素就要找几轮的最小值
    for (let i =0; i < length; i++) {
        //假设第一个是最小的
        let min = arr[i];
        let minIndex = i;
        console.log("in i loop ",min,minIndex);
        //从剩下还没排好的元素开始找最小值
        for (let j = i; j < length; j++) {
            console.log("In deep j loop");
            if( arr[j] < min ) {
                min = arr[j];
                minIndex = j;
                console.log("in deep j loop ",min,minIndex);
            }

        }

        //ES6语法
        [arr[minIndex], arr[i]] = [arr[i], arr[minIndex]];
        console.log(arr); //每次打印出来的结果:第一次找出2 第二次找出5，第三次找出6，依次类推。
    }

    return arr;
};
```

```
console.log(selectionSort([5,6,31,23,9,14,11,2]));
```

在console里可以看到整个过程

```
In i loop  5 0
In deep j loop
In deep j loop
In deep j loop
In deep j loop
In deep j loop
In deep j loop
In deep j loop
In deep j loop
in deep j loop  2 7
[ 2, 6, 31, 23, 9, 14, 11, 5 ] ------ i=0时第一次循环找到最小值。后面的循环就没有2参与
in i loop  6 1                -----6 的索引为1
In deep j loop
In deep j loop
In deep j loop
In deep j loop
In deep j loop
In deep j loop
In deep j loop
in deep j loop  5 7
[ 2, 5, 31, 23, 9, 14, 11, 6 ]------找到最小值2后，找到剩下最小的5
in i loop  31 2               ------31索引2
In deep j loop
In deep j loop
in deep j loop  23 3
In deep j loop
in deep j loop  9 4
In deep j loop
In deep j loop
In deep j loop
in deep j loop  6 7        -------找到6最小
[ 2, 5, 6, 23, 9, 14, 11, 31 ]  ------------从23开始
in i loop  23 3
In deep j loop
In deep j loop
in deep j loop  9 4          ------找到9最小
In deep j loop
In deep j loop
In deep j loop
[ 2, 5, 6, 9, 23, 14, 11, 31 ]
in i loop  23 4
In deep j loop
In deep j loop
in deep j loop  14 5
In deep j loop
in deep j loop  11 6     -------找到11最小
In deep j loop
[ 2, 5, 6, 9, 11, 14, 23, 31 ]
in i loop  14 5         -----刚好到14，没有比14小的，
In deep j loop
In deep j loop
In deep j loop
[ 2, 5, 6, 9, 11, 14, 23, 31 ]
in i loop  23 6       -----刚好到23，没有比23小的
In deep j loop
In deep j loop
[ 2, 5, 6, 9, 11, 14, 23, 31 ]
in i loop  31 7
In deep j loop
[ 2, 5, 6, 9, 11, 14, 23, 31 ]
[ 2, 5, 6, 9, 11, 14, 23, 31 ]  ------最后的输出
```

用到了ES6数组的解构

```
[arr[minIndex], arr[i]] = [arr[i], arr[minIndex]]；
```

在console中测试解构的用法

```
>arr = [5,6,31,23,9,14,11,2];
(8) [5, 6, 31, 23, 9, 14, 11, 2]
>[arr[7],arr[0]] = [arr[0],arr[7]] --这个是第一次排序i=0时找出了最小值2索引为3时最后执行的语句
(2) [5, 2]
>arr
(8) [2, 6, 31, 23, 9, 14, 11, 5]
```

用解构的方法调换两个元素，最小值到了最左边。

然后第二次以i=1再次开始循环。将剩下的最小值放到2右边。

