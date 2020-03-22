## 1. 消失的数字
```javascript
// method1
// 1. 求数组累加的总和
// 2. 循环减去每个元素 剩余即是缺失的整数
// O(n)
var missingNumber = function(nums) {
    const len = nums.length;
    return nums.reduce(function (prev, current) { return prev - current }, (len + 1) * len / 2)
};
```
执行用时 |内存消耗 |语言  
-|-|-
72 ms|	35.8 MB|	Javascript

```javascript
// method2
// 1. 创建一个对象存储出现过的元素
// 2. 再次循环 匹配到不存在的元素即返回
// O(2n)
var missingNumber = function(nums) {
    const dict = nums.reduce((prev, current) => (prev[current] = true, prev), {});
    let index = nums.length + 1;
    while(index--) {
        if (!dict[index]) return index;
    }
};
```
执行用时 |内存消耗 |语言  
-|-|-
64 ms|	37.3 MB|	Javascript



## 2. 下一个排列
```javascript
// 1. 从后往前找到第一个升序位置 并记录升序的第一个元素 matchLeft 及第二个元素 matchRight
// 2. 从后往前找到第一个比 matchLeft 大的元素 并重新赋值 matchRight
// 3. 若没匹配到升序 说明整个数是降序的 直接返回升序排序的最小字典排序数
// 4. 互换 matchLeft 和 matchRight
// 5. 将 matchLeft 后的所有项升序排序
// O(2n + n log n)
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
// 交换数组元素
var swap = function(nums, left, right) {
    let temp = nums[left];
    nums[left] = nums[right];
    nums[right] = temp;
}
// 排序
var fastSort = function(nums, left, right) {
    if (left >= right) return nums;
    const start = left;
    const end = right;
    const pivot = nums[left];
    while (left < right) {
        while (right > left && nums[right] > pivot) {
            right--;
        }
        if (left < right) {
            nums[left] = nums[right];
        }
        while (right > left && nums[left] <= pivot) {
            left++;
        }
        if (left < right) {
            nums[right] = nums[left];
        }
    }
    nums[left] = pivot;
    fastSort(nums, start, left - 1);
    fastSort(nums, left + 1, end);
    return nums;
}
var nextPermutation = function(nums) {
    // 起始位
    let matchLeft = -1;
    let matchRight = -1;
    let match = false;
    for (let i = nums.length; i > 0; i--) {
        // 检查到降序的位置
        if (nums[i - 1] < nums[i]) {
            matchLeft = i - 1;
            matchRight = i;
            match = true;
            break;
        }
    }
    for (let i = nums.length; i > matchLeft + 1; i--) {
        // 从结尾到匹配到的位置 寻找比匹配项大的元素
        if (nums[matchLeft] < nums[i]) {
            matchRight = i;
            break;
        }
    }
    // 如果没有匹配到升序 则返回字典最小排列
    if (!match) return nums.sort((a, b) => a - b);
    swap(nums, matchLeft, matchRight);
    // 对交换的初始位置后的数据进行升序排序
    fastSort(nums, matchLeft + 1, nums.length - 1);
}
```


## 热点题
施工中 👷