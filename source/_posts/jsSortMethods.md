---
title: Javascript 常见排序算法
date: 2020-6-6 17:46:48
tags: 
    - js
categories: 
    - web前端
---

## 快速排序
> 快速排序的基本思想就是分治法的思想，寻找中间点，并对其左右的序列递归进行排序，直到左右都排序完成。

```
  function quickSort(arr){
    if(arr.length==0){
        return arr
    }
    // 寻找中间点的下标
    var pirotIndex=Math.floor(arr.length/2)
    // 移除中间点的元素并且返回这个元素
    var pirot = arr.splice(pirotIndex,1)[0]
    // 定义数组用来存放左右元素
    var left=[],right=[]
    // 开始遍历数组，小于中间点的元素放入左边的子集，大于基准的元素放入右边的子集。
    for(var i=0;i<arr.length;i++){
        if(arr[i]>pirot){
            right.push(arr[i])
        }else{
            left.push(arr[i])
        }
    }
    // 使用递归不断重复这个过程，就可以得到排序后的数组。
    return quickSort(left).concat([pirot],quickSort(right))
  }
  console.log(quickSort([1,3,4,2,8,11,6,9,45]))
```

## 冒泡排序
> 比较相邻的元素。如果第一个比第二个大，就交换他们两个。对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。这样最后的元素就是最大的数。

```
  function bubbleSort(arr) {
    if(arr.length==0){
      return arr
    }
    // 比较相邻的元素，循环 arr.length - 1 次
    var len = arr.length - 1;
    for (var i = 0; i < len; i++) {
      // 每循环一次，内循环次数就减少一次
      for (var j = 0; j < len - i; j++) {
        // 如果前面的数大于后面的数，就交换位置
        if (arr[j] > arr[j + 1]) {
          var temp = arr[j];
          arr[j] = arr[j + 1];
          arr[j + 1] = temp;

          // 这里也可以用ES6的解构来完成交换，[arr[j],arr[j+1]]=[arr[j+1],arr[j]]
        }
      }
    }
    return arr;
  }
  console.log(bubbleSort([1,3,4,2,8,11,6,9,45]))
```