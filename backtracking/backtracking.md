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
- 回溯法可以解决选取组合

```c++
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





