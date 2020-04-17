---
title: leetcode 79.Word Search[Medium] 思路【DFS】
date: 2020-04-02 19:34:58
tags: leetcode
no_toc: true
---

#### 英文原问题
#### Given a 2D board and a word, find if the word exists in the grid.

#### The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

<!-- more -->

#### Example 
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

#### 这道题真的是非常出名的了 很多很多大公司都有考 

#### 直接来吧

#### 我们第一步干什么呢 当然是找given word 的第一个字母出现的地方 比如说given word 是 “SEE” 我们就可以先在这个matrix中找到'S'这个character 毕竟连第一个字母都找不到的话 直接return false就完事了

```java
for(int i = 0; i < board.length; i++){
    for(int j = 0; j < board[0].length; j++){
        if(board[i][j] == word.charAt(0)){
          //找到第一个字母 可以开始了
        }
    }
}
```

#### 然后我们怎么做的 这里的解法是DFS 就是从given word的第一个字母开始不停的探索 上下左右有没有跟着的字母 有的话 继续探索 直到最后一个字母或者中断 在这里我们构建一个dfs的helper function

* 我们需要一个level作为探索的深度 当探索的深度达到given word的长度的时候 其实就可以返回 True 了 因为这证明given word的每一个字母都找到了
* 当然我们需要查所有的 out of index 或者negative index 的cases 如果发现 直接排除
* 然后呢 我们需要把当前字母设为任意一个非字母的character 这样不会出现反复用同一个index 字母的情况
* 然后就是上下左右的搜索啦 只有上下左右有一个找得到 我们就可以继续下一个level 的探索啦
* 代码如下
```java
private boolean dfs(char[][] board, String word, int level, int i, int j){
    if(level == word.length()) 
        return true;
    
    if(i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] != word.charAt(level)) return false;
    
    board[i][j] = '.';
    boolean res = dfs(board, word, level + 1, i + 1, j ) ||
                    dfs(board, word, level + 1, i, j + 1 ) ||
                    dfs(board, word, level + 1, i - 1, j ) || 
                    dfs(board, word, level + 1, i, j - 1 );
    board[i][j] = word.charAt(level);
    
    return res;
}
```

#### 完整代码如下
```java
class Solution {
  public boolean exist(char[][] board, String word) {
    if(board == null || board.length == 0 || board[0].length == 0) return false;
    if(word == null || word.length() == 0) return true;
    
    for(int i = 0; i < board.length; i++){
        for(int j = 0; j < board[0].length; j++){
            if(board[i][j] == word.charAt(0) && dfs(board, word, 0, i, j))
                return true;
        }
    }
    
    return false;
  }
    
  private boolean dfs(char[][] board, String word, int level, int i, int j){
    if(level == word.length()) 
        return true;
    
    if(i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] != word.charAt(level)) return false;
    
    board[i][j] = '.';
    boolean res = dfs(board, word, level + 1, i + 1, j ) ||
                    dfs(board, word, level + 1, i, j + 1 ) ||
                    dfs(board, word, level + 1, i - 1, j ) || 
                    dfs(board, word, level + 1, i, j - 1 );
    board[i][j] = word.charAt(level);
    
    return res;
  }
}
```
