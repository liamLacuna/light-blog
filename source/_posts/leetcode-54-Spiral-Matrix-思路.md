---
title: leetcode 54.Spiral Matrix[Medium] 思路
date: 2020-03-31 17:23:49
tags: leetcode
no_toc: true
---

#### 英文原问题
#### Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

<!-- more -->

#### Example 1
```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

#### Example 2
```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

---

#### 这道题呢 就是一个典型的看起来很难搞但是其实慢慢分开来解决就好了
#### 什么意思呢 我们可以上下左右各设置一个边界， 然后慢慢的按照题目的顺序就提取element出来就好啦

#### 我们先定义一个边界
```java
    int top = 0;
    int bottom = matrix.length - 1;
    int left = 0;
    int right = matrix[0].length - 1;
```

#### 然后我们算一下总共有多少个数字
```java
    int size = matrix.length * matrix[0].length;
```

#### 接下来就很很简单了 跟着题目要求的顺序提取数字
```java
//首先呢 从左到右提
for(int i = left; i <= right && result.size() < size; i++){
    result.add(matrix[top][i]);
}
//然后呢 从最右边开始往下提取
for(int i = top; i <= bottom && result.size() < size; i++){
                result.add(matrix[i][right]);
}
//接着呢 从右下角向左下角提取
for(int i = right; i >= left && result.size() < size; i--){
    result.add(matrix[bottom][i]);
}
//最后呢 从左下角到往上提取
for(int i = bottom; i >= top && result.size() < size; i--){
    result.add(matrix[i][left]);
}
```

#### 接下来我们就返回result就好啦
#### 完整Java代码如下
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<Integer>();
        
        if(matrix == null || matrix.length == 0) return result;
        
        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;
        int size = matrix.length * matrix[0].length;
        
        while(result.size()  < size){
            for(int i = left; i <= right && result.size() < size; i++){
                result.add(matrix[top][i]);
            }
            top++;
            for(int i = top; i <= bottom && result.size() < size; i++){
                result.add(matrix[i][right]);
            }
            right--;
            for(int i = right; i >= left && result.size() < size; i--){
                result.add(matrix[bottom][i]);
            }
            bottom--;
            for(int i = bottom; i >= top && result.size() < size; i--){
                result.add(matrix[i][left]);
            }
            left++;
        }
        
        return result;
    }
}
```
