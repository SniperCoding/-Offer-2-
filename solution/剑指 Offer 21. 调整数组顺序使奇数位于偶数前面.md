# [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

## 题目描述

>输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数在数组的前半部分，所有偶数在数组的后半部分。
>
>**示例：**
>
>```
>输入：nums = [1,2,3,4]
>输出：[1,3,2,4] 
>注：[3,1,2,4] 也是正确的答案之一。
>```
>
>**提示：**
>
>1. `0 <= nums.length <= 50000`
> 2. `0 <= nums[i] <= 10000`

### 想法：双指针

本题要求数组中前面全是奇数，后面全是偶数。

我们可以定义两个指针：

- 一个指针 left 从前往后对数组遍历，用于找偶数；
- 一个指针 right 从后往前对数组遍历，用于找奇数；
- 如果 `left < right`，则交换两个索引处的值
- 如果 `left >= right`，则说明前面全是奇数，后面全是偶数，已经满足要求了！

时间复杂度O( n )，空间复杂度O( 1 )

~~~java
class Solution {
    public int[] exchange(int[] nums) {
        int left = 0;
        int right = nums.length-1;
        while(true){
            while(left<right && nums[left] % 2 != 0){  // 从左边找偶数
                left++;
            }
            while(left<right && nums[right] % 2 == 0){  // 从右边找奇数
                right--;
            }
            // 已经满足要求了！
            if(left>=right){  
                break;
            }
            // 交换两个指针对应的值
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
        }
        return nums;
    }
}
~~~







