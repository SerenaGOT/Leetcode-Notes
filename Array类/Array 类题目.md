

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



##  59. Spiral Matrix II

## 问题描述

Given a positive integer *n*, generate a square matrix filled with elements from 1 to *n*2 in spiral order.

**Example:**

```
Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```



## 代码

```c++
// 本题主要考虑边界条件，不难总体
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n,vector<int>(n,0)); //初始化vector的方法
        int row = n-1,col=n-1;
        int x = 0, y=0;
        int value = 1;
        while(true){
            if(x>col) return res;
            else if(x==col){
                res[x][x] = value;
                return res;
            }
                
            for(int i=y;i<=col;i++){  // to right
                res[x][i] = value++;
            }
            for(int i=x+1;i<=row;i++){
                res[i][col] = value++;
            }
            for(int i=col-1;i>=x;i--){
                res[row][i] = value++;
            }
            for(int i=row-1;i>y;i--){
                res[i][x] = value++;
            }
            x++;
            y++;
            col--;
            row--;
        }
        return res;
    }
};
```





## 167.Two Sum II - Input array is sorted

Given an array of integers that is already **sorted in ascending order**, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

- Your returned answers (both index1 and index2) are not zero-based.
- You may assume that each input would have *exactly* one solution and you may not use the *same* element twice.

**Example:**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

```c++
//解法
// 简单的贪心算法
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int start = 0, end = numbers.size()-1;
        vector<int> solution;
        while(start<end){
            int res = numbers[start]+numbers[end];
            if(res == target){
                solution.push_back(start+1);
                solution.push_back(end+1);
                break;
            }
            else if(res>target){
                end--;
            }
            else if(res<target){
                start++;
            }
        }
        if(numbers[start]+numbers[end]==target)
            return solution;
    }
};
```



## 55. Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

**Example 1:**

```
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

```

**Example 2:**

```c++
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

解法：

```c++
//有点动态规划的意思
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int len=nums.size();
        int maxq=0; // 最多能去到的位置
        int i=0;
        while(i<=maxq&&i<=len-1)
        {
      
            maxq=max(nums[i]+i,maxq); // 当前位置+能走步数，与当前最大能走步数相比，取最大值
           if(i>=len-1)
                return true;
            if(maxq-i<=0)  //如果最大能走步数不能大于i，则失败
                return false;
             i++;
        }
        
    }
};
```



## 80 Remove Duplicates from Sorted Array II

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most *twice* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,1,2,3,3],

Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.

It doesn't matter what values are set beyond the returned length.

```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```



- 解法
  - 这种题都可以考虑在同个数组遍历的时候，后面的元素直接覆盖重复元素的位置

```c++
\\需要考虑到前面遍历过的元素不需要再使用，所以可以直接覆盖
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i=0;
        for(int num:nums){
            if(i<2 || num>nums[i-2]){
                nums[i++] = num;
            }
        }
        return i;
    }
};
```





# 817. Range Sum Query 2D - Mutable

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

1. The matrix is only modifiable by the update function.
2. You may assume the number of calls to update and sumRegion function is distributed evenly.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.

### Example

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```



- 解法：因为如果每次求和都要重新加全部的和会导致超时，所以可以先用一个矩阵储存每一列中0行到j行的和，那么每次计算矩阵的和的时候只需要进行col2-col1次减法就可以得到结果，复杂度从O(n^2)降到了O(n)

```c++
class NumMatrix {
public:
    NumMatrix(vector<vector<int>> matrix) {
        m.resize(matrix.size(),vector<int>(matrix[0].size()));
        sum.resize(matrix.size(),vector<int>(matrix[0].size()));
        if(matrix.size()!=0){
            for(int j=0;j<matrix[0].size();j++){
                for(int i=0;i<matrix.size();i++){
                    m[i][j] = matrix[i][j];
                    if(i==0)
                        sum[i][j] = matrix[i][j];
                    else
                        sum[i][j] = sum[i-1][j]+matrix[i][j];
                }
            }
        }
    }
    
    void update(int row, int col, int val) {
        int dif = m[row][col] - val;
        m[row][col] = val;
        for(int r=row;r<sum.size();r++){
            sum[r][col]-=dif;
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        int res = 0;
        for(int i=col1;i<=col2;i++){
            if(row1>0)
                res+= sum[row2][i] - sum[row1-1][i];
            else
                res+= sum[row2][i];
        }
        return res;
    }
private:
    vector<vector<int>> sum;
    vector<vector<int>> m;
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * obj.update(row,col,val);
 * int param_2 = obj.sumRegion(row1,col1,row2,col2);
 */
```

