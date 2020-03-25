---
title: 逗酱的怪怪编码
date: 2020-03-24 00:51:02
tags:
permalink: doogle-decode
categories:
- 算法
- 动态规划

---

逗酱想要把数字转换成编码。
<!--more-->

## 问题描述

逗酱需要把数字先转换成二进制，再转换成编码，转换规律是：

``` text
0     => 口
00    => 吕
000   => 品
1     => Ⅰ
11    => Ⅱ
111   => Ⅲ
010   => 中
00100 => 串
```

同时逗酱要求转换后的编码长度尽可能小。

## 分析

很基础的动态规划，设二进制字串为s，设dp[i]表示s前i位字符的最小编码长度，则
若s[i]=='0'

+ s[i]单独编码成`口`，编码长度是dp[i-1]+1

+ s[i-1:i]为00，编码成`吕`，编码长度是dp[i-2]+1。

+ s[i-2:i]为000，编码成`品`，编码长度是dp[i-3]+1。

+ s[i-2:i]为010，编码成`中`，编码长度是dp[i-3]+1。

+ s[i-4:i]为00100，编码成`串`，编码长度是dp[i-5]+1。

故dp[i]=min(dp[i-1],dp[i-2]?,dp[i-3]?,dp[i-5]?)+1，注意上述五个情况有些不会满足。

同理若s[i]=='1'

+ s[i]单独编码成`Ⅰ`，编码长度是dp[i-1]+1。

+ 若s[i-1:i]为11，编码成`Ⅱ`，编码长度是dp[i-2]+1。

+ 若s[i-2:i]为111，编码成`Ⅲ`，编码长度是dp[i-3]+1。

故dp[i]=min(dp[i-1],dp[i-2]?,dp[i-3]?)+1，注意上述三个情况不一定同时满足。

最少编码长度就是dp[s.length]

那么知道了最少编码长度，怎么求最少编码呢？

我们可以用left数组标记编码位置，即s[0:i]的最佳编码是s[0:left-1]的编码+s[left:i]的编码。

## 参考代码

```js
const code = {
  '1': 'Ⅰ',
  '11': 'Ⅱ',
  '111': 'Ⅲ',
  '0': '口',
  '00': '吕',
  '000': '品',
  '010': '中',
  '00100': '串'
}

const getBinary = n => {
  if(n===0) return '0'
  let ans = ''
  while (n > 0) {
    ans = n % 2 + ans
    n = Math.floor(n / 2)
  }
  console.log(ans)
  return ans
}

const decodeDoogle = n => {
  let b = getBinary(n)
  let dp = new Array(b.length + 1).fill(0)
  let left = new Array(b.length + 1).fill(0)

  dp[0] = 0

  for (let i = 1; i <= b.length; i++) {
    if (b[i - 1] === '0') {  // 0, 00,000,010,00100
      let p0, p00, p000, p010, p00100
      p0 = dp[i - 1] + 1
      if (i > 1 && b.slice(i - 2, i - 1) === '0') {
        p00 = dp[i - 2] + 1
      }
      if (i > 2 && b.slice(i - 3, i - 1) === '00') {
        p000 = dp[i - 3] + 1
      }
      if (i > 2 && b.slice(i - 3, i - 1) === '01') {
        p010 = dp[i - 3] + 1
      }
      if (i > 4 && b.slice(i - 5, i - 1) === '0010') {
        p00100 = dp[i - 5] + 1
      }

      dp[i] = Math.min(
        p0,
        p00 || Infinity,
        p000 || Infinity,
        p010 || Infinity,
        p00100 || Infinity
      )


      if (p0 === dp[i]) {
        left[i] = i - 1
      }
      if (p00 === dp[i]) {
        left[i] = i - 2
      }
      if (p000 === dp[i] || p010 === dp[i]) {
        left[i] = i - 3
      }
      if (p00100 === dp[i]) {
        left[i] = i - 5
      }

    }
    else {   // 1,11,11 1
      let p1, p11, p111
      p1 = dp[i - 1] + 1
      if (i > 1 && b.slice(i - 2, i - 1) === '1') {
        p11 = dp[i - 2] + 1
      }
      if (i > 2 && b.slice(i - 3, i - 1) === '11') {
        p111 = dp[i - 3] + 1
      }

      dp[i] = Math.min(
        p1,
        p11 || Infinity,
        p111 || Infinity
      )

      if (p1 === dp[i]) {
        left[i] = i - 1
      }
      if (p11 === dp[i]) {
        left[i] = i - 2
      }
      if (p111 === dp[i]) {
        left[i] = i - 3
      }

    }
  }



  let ans = ''
  let p = b.length
  while (p) {
    ans = code[b.slice(left[p], p)] + ans
    p = left[p]
  }
  return ans
}

console.log(decodeDoogle(4870847))
 // 10010100101001010111111
 // => Ⅰ吕Ⅰ中中Ⅰ吕Ⅰ中ⅢⅢ
```
