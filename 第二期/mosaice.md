# 前K个高频单词

常规做法，先group累计出现次数，之后按次数排序输入前K个

执行用时|内存消耗
-|-
76 ms | 37.8 MB
```js
var topKFrequent = function(words, k) {
  var group = words.reduce((p, n) => {
      p[n] ? p[n]++ : p[n] = 1;
      return p
  }, {})

  return Object.keys(group).sort((a, b) => group[b] - group[a] || a.localeCompare(b)).slice(0,k)
};
```

构建优先队列或者是大顶堆的结构来存储词以及出现频率，之后出队K次即可得到结果。但是JS本身没有直接提供优先队列以及大顶堆结构，手写数据结构来特意解决这个问题得不偿失，可以采用优先队列或者大顶堆构筑思想来解题

本质上就是要保证队列结构中次数出现多的一定会出现在最前面，做法上就是在每次入队的时候获取权重值，并在入队后按权重值调整一次

这个代码只实现了入队之后进行统计权重然后重排位置，而且重排用的是数组交换来移动，代码还有优化的空间

- 重排用的交换来做的，这个成本比链表操作大很多（比如从第4位调整到第2位需要交换两次，用双向链表的实现无论隔了多少位只用1次调整）
- 入队的时候会依次查找元素，通过维护一个Set来快速断定是新增还是已有的
- 调整是单调向前的，用堆结构来做可以有效减少比对次数

执行用时|内存消耗
-|-
92 ms | 37.2 MB
```js
var topKFrequent = function(words, k) {
  var queue = Array(words.length)
  var word, item, t, prev
  for (var i = 0; i < words.length; i++) {
      word = words[i]
      for (var j = 0;j < queue.length; j++) {
          item = queue[j]
          if (!item) {
            item = queue[j] = {word: word, priority: 1}
          } else {
            if (item.word === word) {
              item.priority++
            } else {
              continue
            }
          }
          t = j
          while(t > 0) {
              prev = queue[t - 1]
              if (prev.priority < item.priority || (prev.priority === item.priority && item.word.localeCompare(prev.word) < 0)) {
                queue[t - 1] = item
                queue[t] = prev
                t--
              } else {
                break
              }
          }

          break
      }
  }

  for (i = 0;i < k;i++) {
    if (queue[i])
      queue[i] = queue[i].word
  }
  queue.length = k
  return queue
};
```

# 旋转数组

暴力循环法，循环K次，每次移动一位

执行用时|内存消耗
-|-
268 ms | 35.3 MB

```js
var rotate = function(nums, k) {
  var last, temp
  for(var i = 0;i < k; i++) {
      last = nums[nums.length - 1]
      for(var j = 0;j < nums.length;j++) {
          temp = nums[j]
          nums[j] = last
          last = temp
      }
  }
};
```

用新数组保存每个元素到最终位置上，最后输出到原数组

执行用时|内存消耗
-|-
 72 ms | 36.5 MB

```js
var rotate = function(nums, k) {
    var arr = []
    var i = 0;
    for(;i < nums.length;i++) {
        arr[(i + k) % nums.length] = nums[i]
    }

    for (i = 0;i < nums.length;i++) {
        nums[i] = arr[i]
    }
};
```

移动每个元素到K位之后，之后将原本属于该位置的元素往后移动，直到移动N次，表示数组的所有元素都已经移动完毕

执行用时 | 内存消耗
-|-
64 ms | 35.5 MB

```js
var rotate = function(nums, k) {
  var last, current, temp
  k = k % nums.length
  var count = 0
  for(var i = 0;count < nums.length; i++) {
      current = i
      last = nums[i]
      do {
          current = (current + k) % nums.length
          temp = nums[current]
          nums[current] = last
          last = temp
          count++
      } while (current !== i)
  }
};
```

切片法，用额外数组放前半部分和后半部分，之后再组合

执行用时|内存消耗
-|-
116 ms | 36.5 MB

```js
var rotate = function(nums, k) {
  k = nums.length - (k % nums.length)
  var arr1 = []
  var arr2 = []
  for(var i = 0;i < nums.length;i++) {
      if (i < k)
          arr1.push(nums[i])
      else
          arr2.push(nums[i])
  }
  for(var i = 0;i < nums.length;i++) {
      if (arr2.length)
          nums[i] = arr2.shift()
      else if(arr1.length)
          nums[i] = arr1.shift()
  }
};
```

同样思路的，用原生方法优化的速度就是牛逼

执行用时|内存消耗
-|-
64 ms | 35.5 MB

```js
var rotate = function(nums, k) {
    k = nums.length - (k % nums.length)
    nums.unshift(...nums.splice(k))
};
```
# 单点登录 
[博文](https://mosaice.github.io/Blog/#/posts/36)