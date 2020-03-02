### 两数相加：    
#### 第一次的思路是 **先将链表转换为值，求出两个链表的和后，再将数字转化为链表**；  
测试过程中遇到两个问题：  
  1. 当链表长度过长时，会导致精度丢失，解决方法是使用BigInt类型去处理数字；  
  2. 数字转换为链表时，没有使用new操作符，直接调用ListNode导致返回的是undefined

```js
var listToArray = list => {
    var arr = [];
    let cur = list;
    while(cur !== null) {
        arr.push(cur.val);
        cur = cur.next;
    }
    return arr.reverse().reduce((a, b) => BigInt(a) * 10n + BigInt(b), 0n);
}
var arrayToList = value => {
    const arr = `${value}`.split('').map(item => Number(item)).reverse();
    if(!arr.length) return null;
    let cur;
    let head = {val: arr[0], next: null}
    let pre = head;

    for(let i = 1; i < arr.length; i++) {
        cur = {val: arr[i], next: null};
        pre.next = cur;
        pre = cur;
    }
    return head;
}

var addTwoNumbers = function(l1, l2) {
    const value = listToArray(l1) + listToArray(l2);
    return arrayToList(value);
};
```
#### 第二次思路是**直接对链表进行求和**；
测试过程中同样遇到两个问题：  
1. 直接使用head去向下遍历，最后输出的结构始终是链表的最后一个对象  
2. 链表的最后一位相加超过10时，需要进位，代码的while循环需要判断flag是否为1
```js

var addTwoNumbers = function(l1, l2) {
    let head;
    let temHead = head;
    let p1 = l1;
    let p2 = l2;
    let flag = 0;
    while(p1 !== null || p2 !== null || flag) {
        let curVal = flag;
        let curNode;
        if(p1 !== null) {
            curVal += p1.val;
            p1 = p1.next
        }
        if(p2 !== null) {
            curVal += p2.val;
            p2 = p2.next
        }
        if(curVal > 9) {
            flag = 1;
            curVal -= 10;
        } else {
            flag = 0
        }
        curNode = new ListNode(curVal);
        if(temHead) {
            temHead.next = curNode;
            temHead = temHead.next;
        } else {
            head = curNode;
            temHead = head;
        }
    }
    return head
};
```

提交时间 | 提交结果 |执行用时 |内存消耗 |语言  
-|-|-|-|-
[第一次](https://leetcode-cn.com/submissions/detail/49297546/) | 通过 |216 ms |	40 MB |	Javascript  
[第二次](https://leetcode-cn.com/submissions/detail/49315175/) | 通过 |124 ms |	38.3 MB |	Javascript  



### 两数之和    
#### 第一次的思路是 两层循环，遍历元素时找到符合条件的元素；  
```js
var twoSum = function(nums, target) {
    for(let i = 0; i < nums.length; i++) {
        const x = target - nums[i];
        const index=  nums.findIndex(item => item === x);
        if(index !== -1 && index !== i)
            return [i, index];
    } 
};
```
#### 第二次思路是 遍历数组时，以value: index的结构储存在定义的对象中，然后根据target与当前元素的差值匹配对象中的键； 
```js
var twoSum = function(nums, target) {
    const obj = {};
    for(let i = 0; i < nums.length; i++) {
        if(typeof obj[target - nums[i]] === 'number')
            return [obj[target - nums[i]], i]
        else
            obj[nums[i]] = i;
    }
    
};
```
提交时间 | 提交结果 |执行用时 |内存消耗 |语言  
-|-|-|-|-
[第一次](https://leetcode-cn.com/submissions/detail/30854350/) | 通过 |716 ms |	35.6 MB |	Javascript  
[第二次](https://leetcode-cn.com/submissions/detail/49714629/) | 通过 |68 ms  |	34.7 MB |	Javascript  


### 皮肤主题的有哪些实现方式
1. 引用不用的css文件，通过js调整对应的引用文件；  
  - 优点: 简单粗暴，兼容性好
  - 缺点: 需要引用多个文件，维护繁琐
2. 给不同主题的CSS样式设置不同的权重，通过动态调整class的方法来控制主题；  
  - 优点: 兼容性好
  - 缺点: 主题比较多时，维护繁琐，
3. link方式引入less文件，然后使用antd中的modifyVars来修改定义的主题变量；  
  - 优点: 维护方便
  - 缺点: 只支持less，不支持scss
4. 使用css变量来进行主题色的修改，替换主题色变量，然后用setProperty来进行动态修改；  
  - 优点: 操作简单
  - 缺点: 不支持IE

参考链接：
[使用 css/less 动态更换主题色（换肤功能）](https://www.cnblogs.com/leiting/p/11203383.html)
[给网站添加暗黑模式指南](https://mp.weixin.qq.com/s/qfeCcJuJ7eHw-34yCqaSTg)
  