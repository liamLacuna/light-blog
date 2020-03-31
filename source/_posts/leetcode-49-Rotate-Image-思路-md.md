---
title: 'leetcode 49.Rotate Image[Medium] 思路'
date: 2020-02-23 18:40:05
tags: leetcode
no_toc: true
---

#### 英文原问题
#### You are given an n x n 2D matrix representing an image。 Rotate the image by 90 degrees (clockwise).

##### Note:

##### You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

<!-- more -->

#### Example 1
```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

#### Example 2
```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

---

#### 这道题怎么搞呢，首先题目要求我们要做到in-place， 就是不能创建新的array然后一个一个element放进去。我们只能操控这个二维数组本身。咋办呀

#### 我们其实可以先把竖排和横排调换 啥意思呢

```
ex)
1 2 3                   1 4 7
4 5 6            =>     2 5 8
7 8 9                   3 6 9

```

#### 然后再把左边和右边调换就可以了

```
ex)
1 4 7                   7 4 1
2 5 8            =>     8 5 2
3 6 9                   9 6 3
```

#### 完成！

#### 完整Java代码如下
```
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        
        for(int i = 0; i < n; i++){
            for(int j = i; j < n; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            } 
        }
        
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n/2; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - j - 1];
                matrix[i][n - j - 1] = temp;
            }
        }
    }
}
```
