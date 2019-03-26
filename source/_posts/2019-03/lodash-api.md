---
title: Lodash API归档
date: 2019-03-25 14:34:29
tags: Lodash
---

根据函数签名归档的Lodash API（[v4.17.11](https://lodash.com/docs/4.17.11)）。
<!-- more -->
方法如无特别说明则不改变原集合。

# Array

## array -> array(array)

`_.chunk`
按步长分割数组。

## array -> array

`_.compact`
筛除数组中的false,null,0,'',undefined,NAN。

`_.drop`
筛除头部n个元素，默认n=1

`_.dropWhile`
从头部开始筛除元素，直到条件不满足。

`_.dropRight`
筛除尾部n个元素，默认n=1

`dropRightWhile`
从尾部开始筛除元素，直到条件不满足。

`_.flatten`,`_.flattenDeep`,`_.flattenDepth`
扁平处理一层；
递归扁平处理;
扁平处理时指定层数。

`_.initial`
筛除最后一个元素

`_.tail`
筛除第一个元素

`_.slice`
截取数组。默认拷贝完整数组。

`_.take`
截取数组中开始的n个元素，默认n=1。

`_.takeWhile`
从开头往后截取元素，直到条件不满足。

`_.takeRight`
截取数组中倒数的n个元素，默认n=1。

`_.takeRightWhile`
从末尾往前截取元素，直到条件不满足。

`_.uniq`,`_.uniqBy`,`_.uniqWith`
数组去重。

`_.sortedUniq`,`_.sortedUniqBy`
为排好序的数组优化的去重函数。

`_.without`
筛除与指定元素相同的元素。

## ...array -> array

`_.concat`
连接数组。

`_.difference`,`_.differenceBy`,`_.differenceWith`
筛除第一个数组中，同时存在于其余任一数组的元素。

`_.intersection`,`_.intersectionBy`,`_.intersectionWtih`
筛选第一个数组中，同时存在于其余所有数组的元素。

`_.union`,`_.unionBy`,`_.unionWith`
合并数组并去重。

`_.xor`,`_.xorBy`,`_.xorWith`
获取集合的对称差。

## array(...array) -> array(...array)

`_.zip`,`zipWith`
组合多个数组。

`_.unzip`,`_.unzipWith`
解组数组。

## array(array,array) -> object

`_.zipObject`
输入键数组和值数组，返回对象。

`_.zipObjectDeep`
输入键（支持路径）数组和值数组，返回对象。

## array -> number

`_.findIndex`,`_.findLastIndex`

`_.indexOf`

`_.lastIndexOf`

`_.sortedIndex`,`_.sortedIndexBy`
数组必须升序排列。返回二叉搜索value插入的下标的最小值。

`_.sortedLastIndex`,`_.sortedLastIndexOf`
数组必须升序排列。返回二叉搜索value插入的下标的最大值。

`_.sortedIndexOf`
返回升序数组中第一个出现的value的下标。

## array -> element

`_.head`
获取数组第一个元素。

`_.last`
获取数组中最后一个元素。

`_.nth`
获取数组中下标为n的元素，n默认为0。

## array -> string

`_.join`
连接数组元素，默认分隔符为`,`

## pairs -> object

`_.fromPairs`
从包含键值对的数组构建对象。

## object -> pairs

`_.toPairs`
解构对象成包含键值对的数组。

## array -> array 改变原数组

`_.fill`
填充数组，可指定起始点

`_.pull`,`_.pullAll`,`_.pullAllBy`,`_.pullAllWith`
移除与指定元素相同的元素。

`_.pullAt`
移除指定下标的元素。
返回被移除的元素。

`_.remove`
依据指定的规则移除元素。
返回被移除的元素。

`_.reverse`
反转数组。

# Collection

`Collection`是指一个`Array`或者`Object`。

## collection -> collection

`_.forEach`,`_.forEachRight`
遍历。

## collection -> object

`_.countBy`
统计。

`_.groupBy`
分组。

`_.keyBy`
按键分组。

## collection -> boolean

`_.every`
所有元素通过测试。

`_.includes`
集合包含元素。

## collection -> array

`_.filter`
筛选数组或对象的值。

`_.map`
遍历集合，对每个元素调用方法。

`_.flatMap`,`_.flatMapDeep`,`_.flatMapDepth`
对map的结果扁平化。

`invokeMap`
对集合中的每一个元素调用map。

## collection -> element

`_.find`
返回第一个匹配的元素。

`_.findLast`
返回最后一个匹配的元素。