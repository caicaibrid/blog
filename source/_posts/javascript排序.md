---
title: javascript 排序
date: 2016-07-23 10:00:33
categories: Javascript
tags:
     - Javascript
---

# javascript 排序算法

## 冒泡排序

> 比较相邻的两个元素,如果第一个比第二个大,就交换顺序
> 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
> 针对所有的元素重复以上的步骤，除了最后一个。
> 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较

```
function bubbleSort(arr){
  for(var i=0;i<arr.length-1;i++){
    for(var j=0;j<arr.length-1-i;j++){
      if(arr[j]>arr[j+1]){
        var temp = arr[j+1];
        arr[j+1] = arr[j];
        arr[j] = temp;
      }
    }
  }
  return arr
}
```

## 选择排序

> 取到待排序的集合,默认第一个数值已经排序
> 然后从剩余未排序序列继续寻找最小元素,然后放到已排序序列的结尾

```
function selectSort(arr){
	for(var i=0;i<arr.length-1;i++){
		var minIndex = i;
		for(var j=i+1;j<arr.length;j++){
			if(arr[minIndex]>arr[j]){
				minIndex = j;
			}
		}
		var temp = arr[i];
		arr[i] = arr[minIndex];
		arr[minIndex] = temp
	}
	return arr;
}
```

##  插入排序

> 将待排序的序列的地一个元素看作是一个有序序列,,然后从第二个元素到最后一个元素为待排序序列
> 从头到尾依次扫描,将扫描到的每个元素插入有序序列适当的位置

```
function insertSort(arr){
  for(var i = 1;i<arr.length;i++){
  	  var curIndex = i-1;
      var curValue = arr[i]
      while(curIndex >=0 && arr[curIndex]>curValue){
        arr[curIndex+1] = arr[curIndex]
        curIndex--;
	  } 
	  arr[curIndex+1] = curValue
  }
  return arr
}
```

## 二分查找法

> 二分查找法，也为折半查找，首先要找到一个中间值，通过与中间值比较，大的放右，小的放左，再在两边寻找中间值，持续以上操作，直到找到所在位置为止

```
function erfen(arr,num,start,end){
	var start = start || 0;
	var end = end || arr.length-1;
	var middle = Math.ceil((start+end)/2);
	console.log(arr+"-------"+num+"-------"+ middle);
	if(num==arr[middle]){
		console.log("******************")
		return middle;
	}else if(num<arr[middle]){
		return erfen(arr,num,0,middle-1);
	}else{
		return erfen(arr,num,middle+1,false);
	}
}
```

## 快速排序

> 快速排序是冒泡排序的升级，第一趟排序时将数据分成两部分，一部分比另一部分的所有数据都要小。然后递归调用，在两边都实行快速排序。

```
function quickSort(data){
	if(data.length<=1){
		return data;
	}	
	var middle = Math.floor(data.length/2);
	var middleData = data.splice(middle,1)[0];
	
	var left=[];
	var right=[];
	
	for(var i=0;i<data.length;i++){
		if(data[i]<middleData){
			left.push(data[i])
		}else{
			right.push(data[i]);
		}
	}
	console.log(left+"--------"+right)
	return quickSort(left).concat([middleData],quickSort(right))	
}
```