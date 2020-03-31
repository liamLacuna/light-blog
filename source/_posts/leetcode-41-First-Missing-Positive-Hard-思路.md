---
title: 'leetcode 41.First Missing Positive[Hard] 思路'
date: 2020-02-17 17:37:42
tags: leetcode
no_toc: true
---

#### 英文原问题
#### Given an unsorted integer array, find the smallest missing positive integer.

<!-- more -->

#### Example 1
```
Input: [1,2,0]
Output: 3
```

#### Example 2
```
Input: [3,4,-1,1]
Output: 2
```

#### Example 3
```
Input: [7,8,9,11,12]
Output: 1
```

---

#### 如何理解这个问题呢 我第一个想法非常的简单 不就是找从1开始没出现的数字吗 一个for loop的事 怎么会标记为Hard呢 当我蠢蠢的尝试了一下之后 我的天 太多case需要考虑了 



#### 我们一步一步来
#### 首先 我们知道最小的positive integer是1 我们就去看整个array有没有1 没有的话 我们直接return 1就作为我们的最优解和最基本的case
```java
//Base Case
boolean containsOne = false;
for(int i = 0; i < n; i++){    //n = nums.length
    if(nums[i] == 1) {
        containsOne = true;
        break;
    }
}

//如果没有1，可以直接返回1
if(!containsOne) return 1;
```

#### 其中呢 如果array里面既有1 长度n同时也是1的时候 我们可以直接找出2是smallest positive
```java
 if(n == 1) return 2; //nums = [1]
```

#### 接下来讨论下该如何处理另外99.9%的case 如何能更快的既不用额外的data structure又可以很好的找出来缺失的最小正整数呢

#### 我们可以先把所有的负数，所以大于array长度的数和还有数字0 排除 或者把他们变成1 或许可以有其它发现
```java
//把所有负数, 零, 和大于n的数变成1 => 这样的话只剩下正数
for(int i = 0; i < n; i++){
    if((nums[i] <= 0) || (nums[i] > n))
        nums[i] = 1;
}

/* [3, 5, 16, -2, 0, 1, 7, 8, -5, -2, 2, 4]
=> [3, 5, 1, 1, 1, 1, 7, 8, 1, 1, 2, 4] */
```

#### 然后呢 我们可以用标记的方式把从1到n的出现的数字给标出来
#### 数字 => index 然后 array[index] 变成负数 => 这样第一个非负数就是first missing positive 
```
[3, 5, 1, 1, 1, 1, 7, 8, 1, 1, 2, 4]
3 出现 => [3, 5, 1, -1, 1, 1, 7, 8, 1, 1, 2, 4]  arr[3] =>负数
5 出现 => [3, 5, 1, -1, 1, -1, 7, 8, 1, 1, 2, 4]  arr[5] =>负数
1 出现 => [3, -5, 1, -1, 1, -1, 7, 8, 1, 1, 2, 4]  arr[1] =>负数
7 出现 => [3, -5, 1, -1, 1, -1, 7, -8, 1, 1, 2, 4]  arr[7] =>负数
8 出现 => [3, -5, 1, -1, 1, -1, 7, -8, -1, 1, 2, 4]  arr[8] =>负数
2 出现 => [3, -5, -1, -1, 1, -1, 7, -8, -1, 1, 2, 4]  arr[2] =>负数
4 出现 => [3, -5, -1, -1, -1, -1, 7, -8, -1, 1, 2, 4]  arr[4] =>负数
```
#### 这样我们发现从index = 1开始 第6个数字是正数 => 直接返回6就可以了
#### 代码如下
```java
for(int i = 0; i < n; i++){
    int a = Math.abs(nums[i]);
    
    if(a == n)
        nums[0] = -Math.abs(nums[0]);
    else
        nums[a] = -Math.abs(nums[a]);
}

for(int i = 1; i < n; i++){
    if(nums[i] > 0) 
        return i;
}
```

#### 好啦 把所有东西放一起就是最终答案啦
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        
        //Base Case
        boolean containsOne = false;
        for(int i = 0; i < n; i++){
            if(nums[i] == 1) {
                containsOne = true;
                break;
            }
        }
        
        //如果没有1，可以直接返回1
        if(!containsOne) return 1;
        
        if(n == 1) return 2; //nums = [1]
        
        //把所有负数, 零, 和大于n的数变成1 => 这样的话只剩下正数
        for(int i = 0; i < n; i++){
            if((nums[i] <= 0) || (nums[i] > n))
                nums[i] = 1;
        }
        
        //change the sign of ath element if number a is exit
        for(int i = 0; i < n; i++){
            int a = Math.abs(nums[i]);
            
            if(a == n)
                nums[0] = -Math.abs(nums[0]);
            else
                nums[a] = -Math.abs(nums[a]);
        }
        
        for(int i = 1; i < n; i++){
            if(nums[i] > 0) 
                return i;
        }
        
        if(nums[0] > 0)
            return n;
        
        return n + 1;
    }
}
```



