# [剑指 Offer 20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

## 题目描述

>请实现一个函数用来判断字符串是否表示**数值**（包括整数和小数）。
>
>**数值**（按顺序）可以分成以下几个部分：
>
>1. 若干空格
>2. 一个 **小数** 或者 **整数**
>3. （可选）一个 `'e'` 或 `'E'` ，后面跟着一个 **整数**
>4. 若干空格
>
>**小数**（按顺序）可以分成以下几个部分：
>
>1. （可选）一个符号字符（`'+'` 或 `'-'`）
>2. 下述格式之一：
>   1. 至少一位数字，后面跟着一个点 `'.'`
>   2. 至少一位数字，后面跟着一个点 `'.'` ，后面再跟着至少一位数字
>   3. 一个点 `'.'` ，后面跟着至少一位数字
>
>**整数**（按顺序）可以分成以下几个部分：
>
>1. （可选）一个符号字符（`'+'` 或 `'-'`）
>2. 至少一位数字
>
>部分**数值**列举如下：
>
>- `["+100", "5e2", "-123", "3.1416", "-1E-16", "0123"]`
>
>部分**非数值**列举如下：
>
>- `["12e", "1a3.14", "1.2.3", "+-5", "12e+5.4"]`
>
>**示例 1：**
>
>```
>输入：s = "0"
>输出：true
>```
>
>**示例 2：**
>
>```
>输入：s = "e"
>输出：false
>```
>
>**示例 3：**
>
>```
>输入：s = "."
>输出：false
>```
>
>**示例 4：**
>
>```
>输入：s = "    .1  "
>输出：true
>```
>
>**提示：**
>
>- `1 <= s.length <= 20`
>- `s` 仅含英文字母（大写和小写），数字（`0-9`），加号 `'+'` ，减号 `'-'` ，空格 `' '` 或者点 `'.'` 。

## 解法

### 想法：判断整数/小数

本题较为其实挺没意思的，说难不难，说简单却又很繁琐，我们需要按照题目要求，考虑到各种特殊情况的值。

这里给出的思路为：

去除字符串 s 的前后空格，接着遍历一遍字符串  ，找一下有没有 e/E

- 如果没有找到，就判断 s 是不是整数或小数，
  - 如果是，则返回 true
  - 如果不是，则返回 false
- 如果找到了，就判断当前索引前面的字符串是不是整数或小数，以及后面的字符串是不是整数
  - 如果是，则返回 true
  - 如果不是，则返回 false

时间复杂度O( n )，空间复杂度O( 1 )

~~~java
class Solution {
    public boolean isNumber(String s) {
        // 1.去除前后空格
        s = s.trim(); 
        // 2.找到e/E的位置
        int indexOfE = 0;
        for(int i=0;i<s.length();i++){
            if(s.charAt(i)=='e' || s.charAt(i)=='E'){
                indexOfE = i;
                break;
            }
        }
        // 3.判断e/E前后是否满足规则
        if(indexOfE==0){   // 没有e/E
            return isInt(s) || isDecimal(s);
        }else{                // 有e/E
            String s1 = s.substring(0,indexOfE);             // 前面一部分
            String s2 = s.substring(indexOfE+1,s.length());  // 后面一部分
            return (isInt(s1) || isDecimal(s1)) && isInt(s2);
        }
    }

    /**
        判断字符串是不是整数
     */
    public boolean isInt(String s){
        // 特殊情况，s=""或s='+'或s='-'，直接返回false
        if(s.equals("") || s.equals("+") || s.equals("-") ){
            return false;
        }
        // 排除可选的'+'/'-'
        int start = 0;
        if(s.charAt(0)=='+' || s.charAt(0)=='-'){
            start = 1;
        }
        // 判断剩余字符是不是均是数字
        for(int i=start;i<s.length();i++){
            if(!(s.charAt(i)<='9' && s.charAt(i)>='0')){
                return false;
            }
        }
        return true;
    }

    /**
        判断字符串是不是小数
     */
    public boolean isDecimal(String s){
        // 特殊情况，s="" 或 s="+" 或 s="-" 或 s="." 或 s = "+." 或 s="-."，直接返回false
        if(s.equals("") || s.equals("+") || s.equals("-") || s.equals(".") || s.equals("+.") || s.equals("-.")){
            return false;
        }
        // 排除可选的'+'/'-'
        int start = 0;
        if(s.charAt(0)=='+' || s.charAt(0)=='-'){
            start = 1;
        }
        // 判断是不是满足小数规则
        int dotNum = 0;  // 点只能出现一次
        for(int i=start;i<s.length();i++){
            char ch = s.charAt(i);
            if(ch<='9' && ch>='0'){
                continue;
            }else if(ch=='.' && dotNum==0){
                dotNum +=1;
            }else{
                return false;
            }
        }
        return true; 
    }
}
~~~







