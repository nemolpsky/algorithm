## Average of Levels in Binary Tree

Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.
- Example 1:

  Input:
  ```
     3
    / \
   9  20
     /  \
    15   7
  ```

  Output: [3, 14.5, 11]

  Explanation:

  The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].

- Note:
  - The range of node's value is in the range of 32-bit signed integer.
---

## 解题思路
这道题的意思是给定一棵二叉树，要求计算每一层值的平均值。

- 递归

  首先需要一个一个变量来记录每次的递归的深度，根据这个深度来累积每次的值和每次节点的个数，最后再遍历两个集合计算出平均值，时间复杂度是O(n)。

  ```
    public List<Double> averageOfLevels(TreeNode root) {
        // 存储同一层累加数值
        List<Double> valueList = new ArrayList<>();
        // 存储同一层个数
        List<Integer> numList = new ArrayList<>();

        if (root == null) {
            return valueList;
        }
        search(root, 0,valueList,numList);

        // 一层的和除以一层的个数
        for(int i=0;i<numList.size();i++){
            valueList.set(i,valueList.get(i)/numList.get(i));
        }
        return valueList;
    }

    public static void search(TreeNode root, int depth,List<Double> valueList,List<Integer> numList) {
        // 当前节点为空直接返回
        if (root == null) {
            return;
        }

        // 根据当前深度来查看对应索引位置上的个数和数值之和

        // 每一层的第一个节点需要创建默认值
        if (depth == valueList.size()){
            valueList.add(Double.valueOf(root.val));
            numList.add(1);
        }
        // 非一个节点只需要更新值
        else{
            valueList.set(depth,valueList.get(depth)+root.val);
            numList.set(depth,numList.get(depth)+1);
        }

        // 递归调用右节点
        if (root.right != null) {
            search(root.right, depth+1,valueList,numList);
        }

        //递归调用左节点
        if (root.left != null) {
            search(root.left,depth+1, valueList,numList);
        }
    }
  ```