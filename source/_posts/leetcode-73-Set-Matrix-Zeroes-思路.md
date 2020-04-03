---
title: leetcode 73.Set Matrix Zeroes[Medium] 思路
date: 2020-04-01 18:25:33
tags: leetcode
no_toc: true
---

#### 英文原问题
#### Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.

<!-- more -->

#### Example 1
```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

#### Example 2
```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

---
#### 这道题看起来是好像蛮简单的 我一开始想的是只有遍历一次然后看到有0就把对应的整行整列全部标记为0就好啦
#### 我这么做之后 发现结果是基本上全部都变成0了 是因为什么呢

#### 因为其实这是不严谨的 会把不该变0的地方变0 
#### 但如果我们使用标记每个数字的方式 是可以实现的 但是会浪费很多的时间如果这个matrix特别特别大的话

#### 咋办呢

#### 我们可以只标记每个数字对应的第一列和第一行的数字为0就好啦 然后我们可以再遍历一次 只要对应的第一行或者第一列是0 这个数字就会变成0

#### 一步步来

#### 首先 考虑corner case
```
if(matrix == null || 
    matrix.length == 0 || 
    matrix[0].length == 0)
    return;
```

#### 既然我们利用第一行和第一列为标记的用途 我们需要提前知道第一行和第一列中是否存在0 
```
boolean isFirstRowZero = false;
boolean isFirstColZero = false;

int m = matrix.length;
int n = matrix[0].length;


//如果第一行有出现0的情况下， 标记一下
for(int i = 0; i < n; i++){
    if(matrix[0][i] == 0) {
        isFirstRowZero = true;
        break;
    }
}

//如果第一列有出现0的情况下，标记一下
for(int i = 0; i < m; i++){
    if(matrix[i][0] == 0) {
        isFirstColZero = true;
        break;
    }
}
```
#### 然后呢 我们需要遍历一遍这个matrix 并标记下需要变0的行和列
```
//如果某个格子是0的情况下 把这个格子对应的列和对应的行的第一个数字标记为0
for(int i = 1; i < m; i++){
    for(int j = 1; j < n; j++){
        if(matrix[i][j] == 0) {
            matrix[0][j] = 0;
            matrix[i][0] = 0;
        }
    }
}
```

#### 标记完后 只需要遍历第一行和第一列 只要是0 对应的整行整列都会变成0
```
//再一次循环，如果某个数字对应的行或列的第一个数字是0 则这个数字为0
for(int i = 1; i < m; i++){
    for(int j = 1; j < n; j++){
        if(matrix[i][0] == 0 || matrix[0][j] == 0)
            matrix[i][j] = 0;
    }
}
```

#### 最后呢 如果原来的第一行和第一列有0的情况下 把第一行/第一列都变成0
```
//然后就是之前我们的标记 如果标记为0 则这个行全部为0
if(isFirstRowZero){
    for(int i = 0; i < n; i++){
        matrix[0][i] = 0;
    }
}

//同理，如果这个标记为0 则这个列全部为0
if(isFirstColZero){
    for(int i = 0; i < m; i++){
        matrix[i][0] = 0;
    }
}
```

#### 完成啦 
#### 完整代码如下
```
class Solution {
    public void setZeroes(int[][] matrix) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return;
        
        boolean isFirstRowZero = false;
        boolean isFirstColZero = false;
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        
        //如果第一行有出现0的情况下， 标记一下
        for(int i = 0; i < n; i++){
            if(matrix[0][i] == 0) {
                isFirstRowZero = true;
                break;
            }
        }
        
        //如果第一列有出现0的情况下，标记一下
        for(int i = 0; i < m; i++){
            if(matrix[i][0] == 0) {
                isFirstColZero = true;
                break;
            }
        }
        
        //如果某个格子是0的情况下 把这个格子对应的列和对应的行的第一个数字标记为0
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                if(matrix[i][j] == 0) {
                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }
        
        //再一次循环，如果某个数字对应的行或列的第一个数字是0 则这个数字为0
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                if(matrix[i][0] == 0 || matrix[0][j] == 0)
                    matrix[i][j] = 0;
            }
        }
        
        //然后就是之前我们的标记 如果标记为0 则这个行全部为0
        if(isFirstRowZero){
            for(int i = 0; i < n; i++){
                matrix[0][i] = 0;
            }
        }
        
        //同理，如果这个标记为0 则这个列全部为0
        if(isFirstColZero){
            for(int i = 0; i < m; i++){
                matrix[i][0] = 0;
            }
        }
        
        
    }
}
```
