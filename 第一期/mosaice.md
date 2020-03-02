## 第一题

暴力循环去挨个查的思路
时间复杂度 `O(n2)` 空间复杂度 `O(1)`

```js
执行用时：96 ms
内存消耗：34.6 MB
var twoSum = function(nums, target) {
    for (let i=0; i< nums.length; i++) {
        const need = target - nums[i]
        for (let j = i + 1; j < nums.length; j++) {
            if (need === nums[j])
                return [i, j]
        }
    }
    return []
};
```

存储已经遍历过的值与位置，后面遍历中进行反查
时间复杂度 `O(n)` 空间复杂度 `O(n)`

```js
执行用时：108 ms
内存消耗：34.9 MB
var twoSum = function(nums, target) {
    var map = new Map()
    for (var i=0; i< nums.length; i++) {
        var need = target - nums[i]
        if (map.has(need))
            return [map.get(need), i]
        map.set(nums[i], i)
    }
    return []
};

// 优化了下
执行用时：68 ms
内存消耗：34.3 MB
var twoSum = function(nums, target) {
    var map = {}
    var need
    for (var i=0; i< nums.length; i++) {
        need = target - nums[i]
        if (map[need] !== undefined)
            return [map[need], i]
        map[nums[i]] = i
    }
    return []
};
```

## 第二题

遍历链表节点，挨个相加，把多余的位数放到下一位

先放一个最原始的版本，这个版本先考虑相同长度部分的计算问题，长度不等时再做额外的计算


```js
执行用时：140 ms
内存消耗：38.2 MB
var addTwoNumbers = function(l1, l2) {
  var l1Ptr = l1, l2Ptr = l2
  var cur = res = new ListNode(0)
  var v = dight = 0
  var defalutNode = {val: 0}
  while(l1Ptr && l2Ptr) {
    v = l1Ptr.val + l2Ptr.val + cur.val
    dight = ~~(v/10)
    cur.val = v % 10

    if (l1Ptr.next || l2Ptr.next || dight) {
          cur.next = new ListNode(dight)
          dight = 0
          cur = cur.next
      }
      l1Ptr = l1Ptr.next
      l2Ptr = l2Ptr.next
  }


  while(l1Ptr || l2Ptr) {
      l1Ptr = l1Ptr || defalutNode
      l2Ptr = l2Ptr || defalutNode
      v = l1Ptr.val + l2Ptr.val + cur.val
      dight = ~~(v/10)
      cur.val = v % 10
      if (l1Ptr.next || l2Ptr.next || dight) {
          cur.next = new ListNode(dight)
          cur = cur.next
      }
      l1Ptr = l1Ptr.next
      l2Ptr = l2Ptr.next
  }

  return res
};
```
优化了下代码，将位数只当作计算的因素
```js
执行用时：124 ms
内存消耗：38.3 MB
var addTwoNumbers = function(l1, l2) {
  var l1Ptr = l1, l2Ptr = l2
  var cur = res = new ListNode(0)
  var v = dight = 0
  var defalutNode = {val: 0}
  while(l1Ptr || l2Ptr) {
    v = (l1Ptr || defalutNode).val + (l2Ptr || defalutNode).val + dight
    dight = ~~(v/10)
    cur.next = new ListNode(v % 10)
    cur = cur.next

    l1Ptr = l1Ptr && l1Ptr.next
    l2Ptr = l2Ptr && l2Ptr.next
  }

  if (dight)
    cur.next = new ListNode(dight)


  return res.next

};
```

任何遍历都可以改为递归的模式，基于递归的版本
```js
执行用时：136 ms
内存消耗：38.6 MB
var addTwoNumbers = function(l1, l2) {
    if (!l1)
        return l2
    if (!l2)
        return l1
    
    var v = l1.val + l2.val
    var d = ~~(v / 10)
    v = v % 10
    var t = new ListNode(v)
    t.next = d ? addTwoNumbers(new ListNode(d), addTwoNumbers(l1.next, l2.next)) : addTwoNumbers(l1.next, l2.next)

    return t
};
```

在少量数据时，递归额外消耗的栈空间不明显

### 热点题

[水文一篇](https://mosaice.github.io/Blog/#/posts/35)，欢迎讨论、修正


