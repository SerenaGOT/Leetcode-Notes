## HashTable

##  205 Isomorphic Strings

Given two strings **s** and **t**, determine if they are isomorphic.

Two strings are isomorphic if the characters in **s** can be replaced to get **t**.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

**Example 1:**

```
Input: s = "egg", t = "add"
Output: true

```

**Example 2:**

```
Input: s = "foo", t = "bar"
Output: false
```

**Example 3:**

```
Input: s = "paper", t = "title"
Output: true
```



解法：

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        int a[256] = {0};  // 都初始化为0
        int b[256] = {0};
        for(int i=0;i<s.size();i++){
            if(a[s[i]]!=b[t[i]])    
                return false;
            a[s[i]] = i+1;  // 记录这个字母出现的index位置，下一次遇到相同字母比较其上次出现位置是否相同；
            b[t[i]] = i+1;  // 如果a和b的字符不一样则数值不一样
        }
        return true;
    }
};
```





## 274 H-Index

Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index *h* if *h* of his/her *N* papers have **at least** *h* citations each, and the other *N − h* papers have **no more than** *h* citations each."

**Example:**

```
Input: citations = [3,0,6,1,5]
Output: 3 
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had 
             received 3, 0, 6, 1, 5 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.
```

**Note: **If there are several possible values for *h*, the maximum one is taken as the h-index.



解法

```c++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        vector<int> contain(n+1,0);
        //放入桶中
        for(int i=0;i<n;i++){
            if(citations[i]>=n){
                contain[n]++;
            }
            else
                contain[citations[i]]++; // 计算发citations[i]篇文章的数目个数
        }
        int cnt=0;
        for(int i=n;i>=0;i--){
            cnt+=contain[i];   //统计比i要大的文章数目
            if(cnt>=i)
                return i;
        }
        return 0;
        
    }
};
```





## 136 Single Number

Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1

```

**Example 2:**

```
Input: [4,1,2,1,2]
Output: 4
```

解法

```c++
//熟悉c++ map的操作
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        map<int,int> res;
        for(int i=0;i<nums.size();i++){
            if(res.count(nums[i])==0)
                res[nums[i]]=1;
            else
                res[nums[i]]++;
        }
        map<int,int>::iterator it = res.begin();
        for(;it!=res.end();it++){
            if(it->second==1)
                return it->first;
        }
        return -1;
    }
};
```

