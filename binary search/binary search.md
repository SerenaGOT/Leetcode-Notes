# binary search



### 74. Search a 2D Matrix

Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true

```

**Example 2:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

```c++
\\不一定要递归
\\二维数组也能化为一维情况处理
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0 || matrix[0].size()==0)
            return false;
        int row = matrix.size();
        int col = matrix[0].size();
        int start = 0,end = row*col; // 注意边界条件，可以带入小数组试着算一下就知道要不要-1
        while(start<end){
            int mid = (start+end)/2;
            int tmp = matrix[mid/col][mid%col]; // 可以小数组代入计算
            if(tmp>target){
                end = mid;
            }
            else if(tmp<target)
                start = mid+1;
            else{
                return true;
            }
        }
        return false;
    }
};
```



### 81. Search in Rotated Sorted Array II

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).

You are given a target value to search. If found in the array return `true`, otherwise return `false`.

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true

```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

- 解法

```c++
// 最差O(n)的解法，先找到pivot的位置
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int pivot = 0;
        int n = nums.size();
        for(int i=1;i<nums.size();i++){
            if(nums[i]<nums[i-1]){
                pivot = i;
                break;
            }
        } 
        int start = pivot, end = pivot+n;
        while(start<end){
            int mid = (start+end)/2;
            int tmp = nums[mid%n];
            if(tmp<target)
                start = mid+1;
            else if(tmp>target)
                end = mid;
            else{
                return true;
            }
        }
        return false;
    }
};

// log(n) 的解法
class Solution {
public:
  bool search(int A[], int n, int target) {
    int lo =0, hi = n-1;
    int mid = 0;
    while(lo<hi){
          mid=(lo+hi)/2;
          if(A[mid]==target) return true;
          if(A[mid]>A[hi]){  
              if(A[mid]>target && A[lo] <= target) hi = mid; //如果是mid的大于high的，但是区间在lo和mid之间
              else lo = mid + 1;
          }else if(A[mid] < A[hi]){ //mid小于hi的
              if(A[mid]<target && A[hi] >= target) lo = mid + 1;// 如果在 mid和high之间
              else hi = mid;  
          }else{
              hi--; // 若mid和high相同
          }
          
    }
    return A[lo] == target ? true : false;
  }
};
```

### 278. First Bad Version

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example:**

```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
```



- 解法（迭代）

```c++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int start = 1,end = n,mid;
        while(start<end){
            mid = start+(end-start)/2;
            if(!isBadVersion(mid)) start = mid+1;
            else
                end = mid;// 因为mid位置也是bool=1，所以要包含到下一次迭代
        }
        return start;
    }
    
};
```

- 解法（递归，java）

```java
public int firstBadVersion(int n) {
    
    if(n==0) {
        return 0;
    }

   return helper(n,1,n);
}


public int helper(int n, int start, int end) {
    
    if(start>=end) {
        return start;
    }
    int middle = start+(end-start)/2;
    
    if(isBadVersion(middle)) {
        return helper(n,start,middle);
    } else {
        return helper(n,middle+1,end);
        
    }
}
}
```



