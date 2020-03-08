### 前K个高频单词：      
1. 定义对象，以key-value保存所有关键词出现的次数；  
2. 定义比较函数compare；
3. 将value进行排序，并取前k位

```js
function compare(a, b) {
    if(a[1] > b[1])
        return -1
    else if(a[1] < b[1])
        return 1
    else if(a[0] > b[0])
        return 1
    return -1
}
var topKFrequent = function(words, k) {
    var obj = {};
    var length = words.length;
    for(var i = 0; i < length; i++) {
        if(obj[words[i]])
            obj[words[i]] += 1;
        else
             obj[words[i]] = 1; 
    }
    return Object.entries(obj).sort((a, b) => compare(a, b)).slice(0, k).map(item => item[0]);
};
```
提交结果 |执行用时 |内存消耗 |语言  
-|-|-|-
通过 |76 ms |	37.4 MB |	Javascript



### 旋转数组
#### 将k到nums.length-1的元素移到数组的前面； 
1. 使用for循环或while，移除数组最后一位元素并将其添加到第一位，重复k次  
```js
var rotate = function(nums, k) {
    var length = nums.length > k ? k : k % nums.length;
    while(length) {
        nums.unshift(nums.pop());
        length --;
    }
};
```
提交结果 |执行用时 |内存消耗 |语言  
-|-|-|-
通过 |100 ms |	35.5 MB |	Javascript
  
2. 使用变量保存需要移动的k个元素，然后将其移除并添加到数组第一位
```js
var rotate = function(nums, k) {
    var length = nums.length > k ? nums.length - k : nums.length - (k % nums.length);
    var val = nums.splice(length);
    nums.unshift(...val);
};
```
提交结果 |执行用时 |内存消耗 |语言  
-|-|-|-
通过 |68 ms |	35.4 MB |	Javascript


#### 翻转三次数组；  
在nums, 在nums的0到k-1, k到nums.length-1处翻转三次数组

```js
var rotate = function(nums, k) {
    function reverse(start,end) {
        while (start < end) {
            tmp = nums[start];
            nums[start] = nums[end];
            nums[end] = tmp;
            start++;
            end--;
        }
    }
    var length = nums.length > k ? k : k % nums.length;
    if(!length) return;
    var tmp = 0;
    reverse(0,nums.length-1);
    reverse(0,length-1);
    reverse(length,nums.length-1);
};
```
提交结果 |执行用时 |内存消耗 |语言  
-|-|-|-
通过 |68ms |	35.4 MB |	Javascript  Javascript  


### 单点登录
单点登录，主要用于多个应用系统间，用户只需要登录一次就可以访问所有相互信任的应用系统。
根据使用场景可分为：
- 同域名
- 不同子域名
- 完全不同域名

#### 同域名
在多系统位于同域名下时，和单系统登录一样，请求时携带cookie或其他认证方式，后台根据cookie校验登录状态即可

#### 不同子域名
子域名间cookie是不共享的，但各子域名均可获取到父级域名的Cookie，即a.xxx.com与b.xxx.com均可以获取 xxx.com域名下的 cookie。所以可以通过将 cookie 设置在父级域名上，可以达到子域名共享的效果

#### 完全不同域名
不同域名是无法直接共享cookie的,此时引入一个单点登录服务sso；
用户登录成功之后，会与sso认证中心及各个子系统建立会话，用户与sso认证中心建立的会话称为全局会话，用户与各个子系统建立的会话称为局部会话，局部会话建立之后，用户访问子系统受保护资源将不再通过sso认证中心；  
[流程图及以下步骤均源于此文章](https://juejin.im/post/5ca60126f265da30cf176acf#heading-6)  
1. 用户访问系统1的受保护资源，系统1发现用户未登录，跳转至sso认证中心，并将自己的地址作为参数；  
2. sso认证中心发现用户未登录，将用户引导至登录页面；
3. 用户输入用户名密码提交登录申请；  
4. sso认证中心校验用户信息，创建用户与sso认证中心之间的会话，称为全局会话，同时创建授权令牌；  
5. sso认证中心带着令牌跳转会最初的请求地址（系统1）；
6. 系统1拿到令牌，去sso认证中心校验令牌是否有效；
7. sso认证中心校验令牌，返回有效，注册系统1；
8. 系统1使用该令牌创建与用户的会话，称为局部会话，返回受保护资源；
9. 用户访问系统2的受保护资源；
10. 系统2发现用户未登录，跳转至sso认证中心，并将自己的地址作为参数；
11. sso认证中心发现用户已登录，跳转回系统2的地址，并附上令牌；
12. 系统2拿到令牌，去sso认证中心校验令牌是否有效；
13. sso认证中心校验令牌，返回有效，注册系统2；
14. 系统2使用该令牌创建与用户的局部会话，返回受保护资源。



![WX20200307-175014](https://user-images.githubusercontent.com/17400269/76141269-8d00e700-609d-11ea-9180-84b3e3e2718d.png)
