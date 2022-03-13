# [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

## 题目描述

>把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
>
>给你一个可能存在 **重复** 元素值的数组 `numbers` ，它原来是一个升序排列的数组，并按上述情形进行了一次旋转。请返回旋转数组的最小元素。例如，数组 `[3,4,5,1,2]` 为 `[1,2,3,4,5]` 的一次旋转，该数组的最小值为1。 
>
>**示例 1：**
>
>```
>输入：[3,4,5,1,2]
>输出：1
>```
>
>**示例 2：**
>
>```
>输入：[2,2,2,0,1]
>输出：0
>```

## 解法

### 想法：二分搜索

本题一种解法是直接暴力遍历一次数组，找到其中的最小值，时间复杂度为O(n)，但显然不是面试官想要看到的解法。如何进一步降低复杂度呢？一般针对于排序数组的搜索问题，可采用二分搜索，将复杂度降至O(logn)。原始排序数组经过旋转得到 numbers，那么 numbers 可以分为左右两个升序排列的子数组，其中**最小数字就是右子数组的第一个值**。

![](https://cdn.jsdelivr.net/gh/SniperCoding/pictures1/20220313174425.png)

我们首先定义左右指针，分别指向数组的两端：`left=0`，`right=numbers.length-1`

之后可以进行循环的二分搜索，令中间值 `mid = left + (right-left) / 2`，由于Java 的除法是整除，因此有 left <= mid < right，为什么不直接用 mid = (left + right ) / 2 这种解法呢？注意是由于 left + right 的结果可能会超出整数范围，导致整型溢出错误。

我们将中间值与最右侧的值进行比较，分为以下三种情况：

- `numbers[mid]<numbers[right]`：此时说明数组的右半部分[mid,right] 是已排序数组，那么可以直接让 `right=mid` 缩小查找范围，为什么不直接让 right = mid-1 呢，是因为要求的最小数字是右子数组的第一个值，而当前的 mid 可能就是第一个值，如果让 right = mid -1，那么就错过了最小数字！

- `numbers[mid]>numbers[right]`：此时说明数组的右半部分[mid,right]不是已排序数组，那么最小数字一定在[mid+1,right]之中，因此就可以不必考虑数组的左半部分[left,mid]了（此部分一定是升序的），因此可以让 `left = mid+1` 缩小查找范围，为什么此时可以让 left = mid +1 ，而不是 left = mid 呢，因为numbers[mid]>numbers[right]，那么索引 mid 处一定不是最小数字！

- `numbers[mid]==numbers[right]`：数组中允许存在重复数字，此时并不能判断数组右半部分是否已排序，分两种情况：

  - 右半部分是升序数组，那么说明 [mid,right] 中所有值都相等
  - 右半部分不是升序数组，那么说明一定存在数字小于numbers[right]（至少number[right-1]小于number[right]）

  无论哪种情况，可以确定的是 `numbers[right-1]<=numbers[right]`，因此为了缩小查找范围，我们可以另`right=right-1`！！！

一旦 `left == right` 时，此时 numbers[left] = numbers[right] = 最小数字！

时间复杂度O(logn)，空间复杂度O(1)

~~~java
class Solution {
    public int minArray(int[] numbers) {
        int left = 0;
        int right = numbers.length-1;
        while(left<right){
            int mid = left + (right-left)/2;
            if(numbers[mid]<numbers[right]){  // 说明右半边已经排好序
                right = mid;
            }else if(numbers[mid]>numbers[right]){  //说明右半边未排好序，而左半边一定已经排好序
                left = mid + 1;
            }else{      // 并不能说明右半边是否已经排好序，但是可以肯定的是在numbers[right-1]<=numbers[right]，因此可以缩小范围
                right = right-1;
            }
        }
        return numbers[left];  // 走到这里说明原始就是有序的
    }
}
~~~

为什么不使用 numbers[left] 和 numbers[mid] 进行比较呢？

原因是：如果 numbers[left] < numbers[mid]，此时无法缩小查找范围

比如：`[3, 4, 5, 1, 2]` 与 `[1, 2, 3, 4, 5]` ，此时，中间位置的值都比左边大，但最小值一个在后面，一个在前面，因此这种做法不能有效地减治。



