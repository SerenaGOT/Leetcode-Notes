# Stack栈

##  150 Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

**Note:**

- Division between two integers should truncate toward zero.
- The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example 1:**

```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

```

**Example 2:**

```
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

```

**Example 3:**

```
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```



```java
\\解法 java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> figure = new Stack<>();
        int res = 0;
        for(int i=0;i<tokens.length;i++){
            Boolean strResult = tokens[i].matches("(-)?\\d+");  
          // 正则表达，括号()？表示可有可无，\d+表示1或多个数字，两个\为了转义\
            if(strResult)
                figure.add(Integer.parseInt(tokens[i]));
            else if(tokens[i].equals("+")){
                int a = figure.pop();
                int b = figure.pop();
                figure.push(a+b);   
            }
            else if(tokens[i].equals("-")){
                int a = figure.pop();
                int b = figure.pop();
                    figure.push(b-a);
            }
            else if(tokens[i].equals("/")){
                int a = figure.pop();
                int b = figure.pop();
                    figure.push(b/a);
            }
            else if(tokens[i].equals("*")){
                int a = figure.pop();
                int b = figure.pop();
                    figure.push(b*a);
            }
//             else{
//                 figure.add(Integer.parseInt(tokens[i]));
//             }
            
        }
        return figure.peek();
    }
}

```





## 394. Decode String

Given an encoded string, return it's decoded string.

The encoding rule is: `k[encoded_string]`, where the *encoded_string* inside the square brackets is being repeated exactly *k* times. Note that *k* is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, *k*. For example, there won't be input like `3a` or `2[4]`.

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```



```java
// java
public class Solution {
    public String decodeString(String s) {
        String res="";
        Stack<Integer> countStack = new Stack<>();
        Stack<String> resStack = new Stack<>();  //储存之前处理好的结果
        int idx = 0;
        while(idx < s.length()){
            if(Character.isDigit(s.charAt(idx))){ 
                int fig = 0;
                while(Character.isDigit(s.charAt(idx))){
                    fig= fig*10 + (s.charAt(idx++)-'0');
                }
                countStack.push(fig);
            }
            if (s.charAt(idx)=='['){ // 遇到[ 就把之前处理好的结果放入stack中
                resStack.push(res);
                res="";
                idx++;
            }
            else if(s.charAt(idx)==']'){ // 遇到] 就开始处理当前需要重复处理的部分
                StringBuilder temp = new StringBuilder(resStack.pop()); 
                int cnt = countStack.pop();
                for(int i=0;i<cnt;i++){
                    temp.append(res);
                }
                res = temp.toString();
                idx++;
            }
            else{
                res+= s.charAt(idx++);
            }
        }
        return res;
    }
}
```



### 402. Remove K Digits

Given a non-negative integer *num* represented as a string, remove *k* digits from the number so that the new number is the smallest possible.

**Note:**

- The length of *num* is less than 10002 and will be ≥ *k*.
- The given *num* does not contain any leading zero.

**Example 1:**

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

```

**Example 2:**

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

```

**Example 3:**

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```



```c++
class Solution {
    public String removeKdigits(String num, int k) {
        int digits = num.length() - k;  // 剩余位数
        char[] stk = new char[num.length()];
        int top = 0;
        for(int i=0;i<num.length();i++){
            while(top>0 && k>0 && stk[top-1]>num.charAt(i)){  // 遍历的前一位比当前一位数值要大  贪心算法，即若前top个都比当前位置的字符大，则把前面的大的位数都去除掉,以获取剩下的最小值
                top--;
                k--;
            }
            stk[top++] = num.charAt(i);
        }
        int index = 0;
        for(;index<digits;index++){
            if(stk[index]!='0'){
                break;
            }
        }
        String s = "";
        for(int i=index;i<digits;i++){
            s+=stk[i];
        }
        if(index==digits)
            s = "0";
        return s;
    }
}
```

