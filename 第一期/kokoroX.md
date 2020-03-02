### 第一题

将数字分为大于目标值一半及小于目标值一半的两个数组进行循环求和

```javascript
执行用时 132 ms
内存消耗 36.3 MB
var twoSum = function(nums, target) {
    // 最小数
    const minimum = Math.min(...nums);
    const distance = minimum > 0 ? 0 : Math.abs(minimum);
    const handledNums = nums.map((num, index) => ({ index: index, value: num }))
    const list = handledNums.filter(item => target + Math.abs(minimum) >= item.value);
    const total = target - distance;
    const half = target < 0 ? target / 2 : target - total / 2;

    const halfLess = [];
    const halfLarge = [];
    const equalList = [];

    for (var i = 0; i < list.length; i++) {
        if (list[i].value < half) halfLess.push(list[i]);
        if (list[i].value >= half) halfLarge.push(list[i]);
        if (list[i].value === target / 2) equalList.push(list[i]);
    }

    if (equalList.length === 2) return [equalList[0].index, equalList[1].index];

    for (var i = 0; i < halfLess.length; i++) {
        for (var j = 0; j < halfLarge.length; j++) {
            if (halfLess[i].value + halfLarge[j].value === target) {
                return [halfLess[i].index, halfLarge[j].index].sort();
            }
        }
    }
};
```

### 第二题

按位相加，若有进位存储标记

```javascript
执行用时 144 ms
内存消耗 39.1 MB
var addTwoNumbers = function(l1, l2) {
    let [n1, n2] = [l1, l2];
    let forward = false;
    let last = null;
    let result = null;
    while(n1 || n2) {
        const n1Val = n1 ? n1.val : 0;
        const n2Val = n2 ? n2.val : 0;
        // 单位数字和
        let sum = n1Val + n2Val;
        // 若存了进位标记 则加一
        if (forward) sum++;

        const currentLast = {
            val: sum >= 10 ? sum - 10 : sum,
            next: null
        };
        if (!last) {
            last = currentLast;
        } else {
            last.next = currentLast;
            last = last.next;
        }
        if (!result) result = last;
        forward = sum >= 10;

        n1 = n1 && n1.next;
        n2 = n2 && n2.next;
    }
    if (forward) last.next = { val: 1, next: null };

    return result;
};
```

## 热点题

[https://www.yuque.com/xujiadong/wh1mf8/owhnif](https://www.yuque.com/xujiadong/wh1mf8/owhnif) 还未完成 先占个坑