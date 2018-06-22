Leetcode:evaluate-reverse-polish-notation

## 题目描述

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are+,-,*,/. Each operand may be an integer or another expression.

Some examples:

```
  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
```
一个逆波兰式，包括+，-，*，/，运算，计算逆波兰式

分析

就是计算逆波兰式的一个算法，所谓逆波兰式，就是操作数在操作符前面，只要将将逆波兰式转换为正常的运算即可，用栈操作即可，遇到加减乘除则出栈，由于全是两则运算，每次出栈两次，得到 x，y ，按照对应符号进行运算即可，算完接着入栈

### 考察点

栈，逆波兰式

### 代码

```
class Solution {
public:
    int evalRPN(vector<string> &tokens) {
        stack <int> stackResult;
        int x,y;
        for(int i=0;i<tokens.size();i++){
            string s = tokens[i];
            if(s=="+" || s=="-" || s=="*" || s=="/"){
                if(stackResult.size()<2) return 0;
                y = stackResult.top();stackResult.pop();
                x = stackResult.top();stackResult.pop();
                if(s=="+")x = x+y;
                else if(s=="-")x = x-y;
                else if(s=="*")x = x*y;
                else x = y==0 ? 0:x/y;
                stackResult.push(x);
            }else{
                stackResult.push(atoi(s.c_str()));
            }
        }
        return stackResult.top();
    }
};
```

