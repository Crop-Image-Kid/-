## 前K个高频单词
```javascript
// method 1
// 1. 算出每个 word 对应的出现次数
// 2. 遍历对象中的每一个 key 并根据 次数和字母大小排序
// 时间复杂度 时间复杂度 O(2n + sort)
var compareWords = function(word1, word2) {
    return word1 > word2 ? 1 : -1;
};
var topKFrequent = function(words, k) {
    const wordCountMap = words.reduce((prev, current) => {
        if (!prev[current]) {
            prev[current] = { value: 0, label: current };
        }
        prev[current].value++;
        return prev;
    }, {});
    return Object.keys(wordCountMap).sort((a, b) => wordCountMap[a].value === wordCountMap[b].value ? compareWords(wordCountMap[a].label, wordCountMap[b].label) : wordCountMap[b].value - wordCountMap[a].value).slice(0, k);
};
```
执行用时|内存消耗|语言
-|-|-
80 ms	|36.6 MB| Javascrip

## 旋转数组
```javascript
// method 1
// 循环将最后一个加到最前面
// 时间复杂度 O(n)
var rotate = function(nums, k) {
    while (k--) {
        nums.unshift(nums.splice(nums.length - 1, 1));
    }
    return nums;
};
```
执行用时|内存消耗|语言
-|-|-
120 ms	|37.3 MB| Javascrip

```javascript
// method 2
// 时间复杂度 O(2)
// 移动 nums.length - k 到 nums.length 的数组到 nums 开头
var rotate = function(nums, k) {
    nums.unshift.apply(nums, nums.splice(nums.length - k));
    return nums;
};
```
执行用时|内存消耗|语言
-|-|-
112 ms	|35.3 MB| Javascrip

```javascript
// method 3
// 时间复杂度 O(2)
// 思路与上一种一样 实现不一样
var rotate = function(nums, k) {
    nums.splice.apply(nums, [0, 0].concat(nums.splice(nums.length - k)));
    return nums;
};
```
执行用时|内存消耗|语言
-|-|-
76 ms	|35.2 MB| Javascrip


> [单点登录](https://www.yuque.com/xujiadong/wh1mf8/coeek2) 施工中