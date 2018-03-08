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



