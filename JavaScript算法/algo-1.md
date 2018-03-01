# JavaScript算法
JavaSript实现快速排序
参考资料
[快速排序](http://natumsol.github.io/2016/02/20/sort4/)

对于待排数组S，快速排序可以简单的描述为：
1. 如果S中的元素个数为0或1，则返回。
2. 取S中任意一元素v，称之为枢纽元（pivot）。
3. 将$S-\{v\}$(S中的其余元素)分成两个不相交的集合：$S_1= \{x \in S - \{v\} | x \leq v\}$ 和 $S_2= \{x \in S - \{v\} | x \geq v\}$。
4. 返回quick_sort(S1)后，继随v，继而quick_sort(S2)。

算法平均时间复杂度为：$O(NlogN)$

JavaScript版快速排序实现
```JavaSript
function swap(data, pos1, pos2){
    var temp = data[pos1];
    data[pos1] = data[pos2];
    data[pos2] = temp;
}
function media3(data, left, right) {
    var center = Math.floor((left + right) / 2);
    if(data[left] > data[center]) {
        swap(data, left, center);
    }
    if(data[left] > data[right]) {
        swap(data, left, right);
    }
    if(data[center] > data[right]) {
       swap(data, center, right);
    }   
    swap(data, center, right - 1);
    return data[right - 1];
}
function quick_sort(data, left, right){
    if(left + 3 <= right) {
        var center = Math.floor((left + right) / 2);
        var pivot = media3(data, left, right), i = left, j = right - 1;
        while(1){
            while(data[++ i] < pivot) {}
            while(data[-- j] > pivot) {}
            if(i < j) {
                swap(data, i , j);
            } else {
                break;
            }
        }
        swap(data, center, right - 1);
        quick_sort(data, left, i - 1);
        quick_sort(data, i + 1, right);
    } else {
        insert_sort(data, left, right);
    }
}
function insert_sort(data, left, right){
    if(left < right){
        for(var i = 1; i < right - left + 1; i ++) {
            var temp = data[i];
            for(var j = i; j > 0 && data[j - 1] > temp; j --) {
                    data[j] = data[j - 1];
            }
            data[j] = temp;
        }
    }
    return data;
}
```
