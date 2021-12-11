---
title: LRU算法
date: 2020-06-29 00:55:46
tags:
 - 算法
---

实现一个最近最少使用缓存机制。

<!--more-->

[前端进阶算法3：从浏览器缓存淘汰策略和Vue的keep-alive学习LRU算法](https://mp.weixin.qq.com/s/mEoP1Ukkvo4MhqrRNJ_Abw)
[146. LRU缓存机制 - 力扣（LeetCode）](https://leetcode-cn.com/problems/lru-cache/)
## 问题
设计一个LRU缓存机制：
+ get(key) 操作，如果 key 存在于缓存中，则返回值，否则返回-1。
+ set(key,value) 操作，如果 key 存在于缓存中，则更新值，否则创建该组[key/value]。若缓存容量达到 capacity ，则去除最近最少使用的缓存节点，以腾出空间。

## 分析
用一个哈希表存储数据，每一个节点有如下属性
```js
class Node{
    key
    value
    prev
    next
}
```
prev 和 next 用于组织节点成双向链表。
链表头部为最长时间未被使用节点，链表尾部为最近（上一次）被使用节点。
读取 key 并且命中，或者 set 操作时，将命中节点放到队尾。
需要清除LRU节点时，移除队首节点。为了根据双向链表中的位置（head.next）找到Map中的位置，创建节点时，记录节点在Map中的key。

## 示例代码
```js
class Node{
    key
    value
    prev
    next
    constructor(key,value){
        this.key=key
        this.value=value
    }
}
 
const link=(nodeA,nodeB)=>{
    nodeA.next=nodeB
    nodeB.prev=nodeA
}

class LRUCache{
    cache=new Map()
    capacity
    head=new Node()                         //head表示最少访问
    constructor(capacity){
        this.capacity=capacity
        link(this.head,this.head)
    }
    moveToTail(cache_at_key){       //tail表示最近访问
        //优化：连续访问时无需操作
        if(cache_at_key.next===this.head) return;
        link(cache_at_key.prev,cache_at_key.next)
        let tail_prev_node=this.head.prev
        link(tail_prev_node,cache_at_key)
        link(cache_at_key,this.head)
    }
    get(key){
        let cache_at_key=this.cache.get(key)
        if(cache_at_key){
            //命中，放到最近访问
            this.moveToTail(cache_at_key)
            return cache_at_key.value
        }
        else{
            return -1
        }
    }
    put(key,value){
        let cache_at_key=this.cache.get(key)
        if(cache_at_key){
            cache_at_key.value=value
            //命中，放到最近访问
            this.moveToTail(cache_at_key)
        }
        else{                
            if(this.cache.size===this.capacity){
                let lru_node=this.head.next
                link(this.head,lru_node.next)
                //淘汰LRU
                this.cache.delete(lru_node.key)
            }
            //创建节点，并放到最近访问
            let cache_at_key=new Node(key,value)
            this.cache.set(key,cache_at_key)
            link(this.head.prev,cache_at_key)
            link(cache_at_key,this.head)
        }
    }
}
```