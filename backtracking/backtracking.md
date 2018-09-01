# backtracking

### 77. Combinations

Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.

**Example:**

```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```



解法：

```c++
//注意回溯的过程需要把之前放入的数弹出
class Solution {

public:

    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>>  res;
        vector<int> obj;
        procedure(0,n,k,obj,res);
        return res;
    }
    void procedure(int start,int n,int k,vector<int>& obj,vector<vector<int>>& res){
        if(obj.size()==k){
            res.push_back(obj);
            return ;
        }
        for(int i=start;i<n;i++){
            obj.push_back(i+1);
            procedure(i+1,n,k,obj,res);
            obj.pop_back();
        }
    }

};


```



### 216. Combination Sum III

Find all possible combinations of **\*k*** numbers that add up to a number **\*n***, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note:**

- All numbers will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]

```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```



- 解法

```c++
//回溯法可以解决选取组合的问题
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> res;
        vector<int> obj;
        combination(k,n,res,obj);
        return res;
    }
    void combination(int k,int n,vector<vector<int>>& res, vector<int>& obj){
        if(obj.size()==k && n==0){
            res.push_back(obj);
            return ;
        }
        for(int i=(obj.empty()?1:obj.back()+1);i<=9;i++){
            if(n-i<0) break;
            obj.push_back(i);
            combination(k,n-i,res,obj);
            obj.pop_back();
        }
    }
};
```





## 90 Subsets II

Given a collection of integers that might contain duplicates, **nums**, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```



- 解法
- 参考 https://leetcode.com/problems/subsets-ii/discuss/30168/C++-solution-and-explanation

```c++
// 对于重复字符,要对整组重复数字进行排列组合处理(出现0次，1次，2次，3次。。。)
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> res;
        int prev_nums = 1;
        int cnt=0; // count the times of same num
        sort(nums.begin(),nums.end());
        vector<int> tmp;
        res.push_back(tmp);
        for(int i=0;i<nums.size();){
            int cnt = 0;
            while(cnt + i<nums.size() && nums[cnt+i] == nums[i]) cnt++;  // count the times of same num,at lease cnt = 1 to quit
            for(int x=0;x<prev_nums;x++){
                vector<int> instance = res[x];
                for(int j=0;j<cnt;j++){
                    instance.push_back(nums[i]);
                    res.push_back(instance);
                }
            }
            prev_nums = res.size();   // 之前的组合之上再加入新的值
            i+=cnt;// 跳过重复的字符
        }
        return res;
        
    }
};
```



### 861. Score After Flipping Matrix

We have a two dimensional matrix `A` where each value is `0` or `1`.

A move consists of choosing any row or column, and toggling each value in that row or column: changing all `0`s to `1`s, and all `1`s to `0`s.

After making any number of moves, every row of this matrix is interpreted as a binary number, and the score of the matrix is the sum of these numbers.

Return the highest possible score.

 


**Example 1:**

```
Input: [[0,0,1,1],[1,0,1,0],[1,1,0,0]]
Output: 39
Explanation:
Toggled to [[1,1,1,1],[1,0,0,1],[1,1,1,1]].
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39
```



- 解法

```c++
\\贪心算法，需要反转后得到最大值，即矩阵最左边都要为1(比右边所有1加起来都要大)，每一行的值权重相同，之后在考虑每一列中需要更多的1s的数目
class Solution {
public:
    int matrixScore(vector<vector<int>>& A) {
        // first make sure each row has the maximum 1 on the left to be biggest
        for(int i=0;i<A.size();i++){
            if(A[i][0]==0){
                for(int j=0;j<A[i].size();j++){
                    A[i][j]^=1;
                }
            }
        }
        // then get more 1s in each col
        for(int i=1;i<A[0].size();i++){
            int cnt=0;
            for(int x = 0;x<A.size();x++){
                cnt+=A[x][i];
            }
            if(cnt < A.size()-cnt){
                for(int x = 0;x<A.size();x++){
                    A[x][i]^=1;
                }
            }
        }
        // count the sum of each row
        int res = 0;
        for(int i=0;i<A.size();i++){
            for(int j=A[0].size()-1;j>=0;j--){
                res+=pow(2,A[0].size()-1-j)*A[i][j];
            }
        }
        return res;
    }
};
```



### 40. Combination Sum II

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]

```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

- 解法
  - 要注意本题有重复的元素，所以在递归的过程中，在拥有相同元素的递归层中，需要避免丢出一个元素，再进入一个相同的元素，相同的元素只能在下一层递归中出现。

```c++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        vector<vector<int>> res;
        vector<int> obj;
        comb(candidates,res,obj,0,target);
        return res;
    }
    void comb(vector<int>& candidates, vector<vector<int>>& res, vector<int> obj, int index,int target){
        if(target==0){
            res.push_back(obj);
            return;
        }
        for(int i=index;i<candidates.size();i++){  
            if(i>index && candidates[i] == candidates[i-1]) continue; // 确保每次递归的时候，相同个数元素的时候同一个元素不会放入放出两次
            if(target-candidates[i]>=0){
                obj.push_back(candidates[i]);
                comb(candidates,res,obj,i+1,target-candidates[i]);
                obj.pop_back();
            }
        }
    }
};
```

