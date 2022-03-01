# [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

## 题目描述

>找出数组中重复的数字。
>
>
>在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>
>**示例 1：**
>
>```sh
>输入：
>[2, 3, 1, 0, 2, 5, 3]
>输出：2 或 3 
>```
>
>**限制：**
>
>```
>2 <= n <= 100000
>```

## 解法

### 想法1：HashSet集合

Set集合可以很方便的判断一个数是否重复出现过，因此可以通过遍历数组，不断将数组中的元素放入到Set集合中，一旦发现重复的情况，则返回此重复数字。

时间复杂度O(n)，空间复杂度O(n)

~~~java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int i=0;i<nums.length;i++){
            if(!set.add(nums[i])){		// 出现重复数字即返回
                return nums[i];
            }
        }
	    return -1;
    }
}
~~~

### 想法2：额外数组

由于题目中已经规定所有数字均在0～n-1 的范围内,那么可以额外使用一个数组arr（类型为布尔值或者整型均可），长度为n，以布尔型数组为例，其下标就代表数字。遍历一遍数组，如果arr[当前数字]为true，则说明之前出现过，返回当前数字即可，否则将arr[当前数字]设置为true，继续往后遍历。

时间复杂度O(n)，空间复杂度O(n)

~~~java
class Solution {
    public int findRepeatNumber(int[] nums) {
        boolean[] arr = new boolean[nums.length];
        for(int i=0;i<nums.length;i++){
            if(arr[nums[i]]){		// 出现重复数字即返回
                return nums[i];
            }
            arr[nums[i]] = true;
        }
        return -1;
    }
}
~~~



