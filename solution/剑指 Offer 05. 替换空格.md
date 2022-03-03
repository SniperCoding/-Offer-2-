# [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

## 题目描述

>请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。
>
>**示例 1：**
>
>```
>输入：s = "We are happy."
>输出："We%20are%20happy."
>```
>
>**限制：**
>
>```
>0 <= s 的长度 <= 10000
>```

## 解法

### 想法：使用StringBuilder

遍历字符串，如果遍历的字符不为空格，直接将其添加到 StringBuilder 中，如果是空格，则把 “20%” 添加到 StringBuilder 中。 

时间复杂度O(n)，空间复杂度O(n)

~~~java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<s.length();i++){
            char c = s.charAt(i);
            if(c==' '){
                sb.append("%20");
            }else{
                sb.append(c);
            }
        }
        return sb.toString();

    }
}
~~~



