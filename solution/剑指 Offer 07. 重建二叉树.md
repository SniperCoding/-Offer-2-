# [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

## 题目描述

>输入某二叉树的前序遍历和中序遍历的结果，请构建该二叉树并返回其根节点。
>
>假设输入的前序遍历和中序遍历的结果中都不含重复的数字。
>
>**示例 1:**
>
>![](https://cdn.jsdelivr.net/gh/SniperCoding/pictures1/20220308213811.png)
>
>```
>Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
>Output: [3,9,20,null,null,15,7]
>```
>
>**示例 2:**
>
>```
>Input: preorder = [-1], inorder = [-1]
>Output: [-1]
>```
>
>**限制：**
>
>```
>0 <= 节点个数 <= 5000
>```

## 解法

### 想法：递归

前序遍历：`根左右`

中序遍历：`左根右`

根据前序遍历和中序遍历的特点，可以对前序遍历数组进行遍历，找到当前遍历的值在中序遍历数组中的位置，利用此值构建节点，<u>那么在此位置之前的值一定是此节点的左子树，而在此位置之后的值一定是当前节点的右子树。</u>

为了能够可以方便的找到比遍历值在中序遍历数组中的索引位置，可以建立一个HashMap，记录值与索引的关系（注意题目说明了不含重复的数字）

时间复杂度O(n)，空间复杂度O(n)

~~~java
class Solution {

    Map<Integer,Integer> mapInOrder;   // 键为中序遍历的值，值为其索引
    int index;  					   // 存储当前遍历的前序索引值
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // 初始化中序遍历数组中值与索引的映射关系
        mapInOrder = new HashMap<>();
        index = 0;
        for(int i=0;i<inorder.length;i++){
            mapInOrder.put(inorder[i],i);
        }
        return buildTree(preorder,0,preorder.length);
    }

    /**
        preorder 是前序遍历结果
        begin是中序索引起始点
        end是中序索引终止点
     */
    public TreeNode buildTree(int[] preorder,int begin,int end){
        if(index>=preorder.length || begin>end){
            return null;
        }
        int current = preorder[index++];  // 当前要处理的值
        int inorderIndex =  mapInOrder.get(current);  // 当前要处理的值在中序遍历中的索引,那么其左边的为左子树，右边的为右子树

        // 构建当前节点以及其左右子树
        TreeNode node = new TreeNode(current);
        node.left = buildTree(preorder,begin,inorderIndex-1);
        node.right = buildTree(preorder,inorderIndex+1,end);
        return node;
    }
}
~~~

