## Range Sum of BST

Given the root node of a binary search tree, return the sum of values of all nodes with value between L and R (inclusive).

The binary search tree is guaranteed to have unique values.

- Example 1:

  Input: root = [10,5,15,3,7,null,18], L = 7, R = 15

  Output: 32

- Example 2:

  Input: root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10

  Output: 23
 

- Note:

  The number of nodes in the tree is at most 10000.
  The final answer is guaranteed to be less than 2^31.

- Definition for a binary tree node
  ```
   public class TreeNode {
       int val;
       TreeNode left;
       TreeNode right;
       TreeNode(int x) { val = x; }
   }
  ```
---

## 解题思路
这道题本身并不难，难点在于理解题意和对BST的结构理解，BST是二叉搜索树，它的特点是所有节点的左枝都比当前节点小，右枝都比当前大。上面TreeNode类就是在Java中代码的结构，下面就是上述两个例子的树状图了，而这道题目是给出左右两枝中的一个点，求两个点之间的值的和(不包括两点自己)，而且左右点之间的点一定是比左枝点大，而比右枝点小。
```
// [10,5,15,3,7,null,18]  32
                   10
                /      \
              5          15
            /   \      /    \
          3      7  null      18
```
```
// [10,5,15,3,7,13,18,1,null,6] 23
                   10
                /      \
              5          15
            /   \      /    \
          3      7   13      18
        /  \    /
       1  null 6
```

- 遍历所有节点进行判断

  - 既然已经知道求的值是要满足什么要求，所以只需要遍历所有的节点判断就可以了。从根节点开始，如果比左点大而比右点小就表示符合要求，需要加上这个节点的值，下面是使用递归来遍历节点。

    ```
    int count = 0;
    public int rangeSumBST(TreeNode root, int L, int R) {
        // 判断节点是否为空
        if(root==null){
            return 0;
        }
        
        // 判断节点的值是否符合要求
        int val = root.val;
        if (L<=val && R>=val){
            count += root.val;
        }

        // 递归调用节点下的左右节点
        rangeSumBST(root.left, L, R);        
        rangeSumBST(root.right, L, R);

        return count;
    }
    ```
  - 上述方法其实还有改进的地方，比如第一个例子，第一个节点值是10，左值是7，右值是15，既然已经判断出它在左值7的右边了，它的右节点也一定是比7大也一定是在右边，就不需要判断，只需要判断10的左节点即可，同理对右值15的判断也是如此，这样一来所有的节点都只会遍历一次，所以时间复杂度是树的大小，也就是O(n)
    ```
    // 递归调用节点下的左右节点
    if(L < val){
        rangeSumBST(root.left, L, R);        
    }

    if(R > val){
        rangeSumBST(root.right, L, R);
    }
    ```
