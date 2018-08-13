# Array 类题目

## 79. Word Search

### 问题描述

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

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



### 代码

```c++
// 匹配在array中字符类似于找到迷宫路径，且前进方向任意，需要在判定当前位置之后标记位置已读，并在判定结束后还原该字符，最后通过递归找到匹配的项
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[i].size();j++){
                if(check(word,0,i,j,board))
                    return true;
            }
        }
        return false;
    }
    bool check(string word,int p,int i,int j,vector<vector<char>>& board){
        if(p==word.size())// 边界退出条件
            return true;
        if(i<0 || j<0 || i>board.size()-1 || j>board[0].size()-1)
            return false;
        if(board[i][j]==word[p]){
            board[i][j] = '*'; // avoid duplicate search
            bool judge = check(word,p+1,i+1,j,board) || check(word,p+1,i-1,j,board) || check(word,p+1,i,j+1,board) || check(word,p+1,i,j-1,board); // 每个方向都试探
            board[i][j] = word[p];  // change back
            return judge;
        }
        else
            return false;
    }
};
```

