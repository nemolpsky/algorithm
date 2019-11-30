## Minimum Distance Between BST Nodes


Given a Binary Search Tree (BST) with the root node root, return the minimum difference between the values of any two different nodes in the tree.

- Example :

  Input: root = [4,2,6,1,3,null,null]

  Output: 1

  Explanation:

  Note that root is a TreeNode object, not an array.

  The given tree [4,2,6,1,3,null,null] is represented by the following diagram:

  ```
          4
        /   \
      2      6
     / \    
    1   3  
  ```
  
  while the minimum difference in this tree is 1, it occurs between node 1 and node 2, also between node 3 and node 2.

- Note:

  - The size of the BST will be between 2 and 100.
  - The BST is always valid, each node's value is an integer, and each node's value is different.

---

## 解题思路
这道题目的意思是找出二叉搜索树中两个节点绝对值最小的数值。

- 数组保存节点值，然后对比

  先深度优先搜索整个节点，存储每个节点的值，再逐个对比这些值的差值，时间复杂度是O(n)

  ```
    public int minDiffInBST(TreeNode root) {
        int[] arr = new int[1000];

        // 深度优先搜索存储所有节点的值
        search(root, arr);

        int min = Integer.MAX_VALUE;
        int start = 0;
        int end = 0;
        // 遍历集合，对比非零数字之间的差值，找出最小值
        for (int i = 0; i < arr.length - 1; ) {
            if (start<arr.length-1) {
                while (arr[start] == 0) {
                    start++;
                }
                end = start + 1;
                while (arr[end] == 0) {
                    end++;
                }
                min = Math.min(Math.abs(arr[end] - arr[start]), min);
                i = end + 1;
                start = end;
            }
        }

        return min;
    }

    public void search(TreeNode root, int[] arr) {
        if (root == null) {
            return;
        }

        arr[root.val] = root.val;

        search(root.left, arr);
        search(root.right, arr);

    }

  ```

- 深度优先搜索

  因为题目给的树是二叉搜索树，所以如果直接从根节点开始深度优先搜索，遍历的值刚好就是一个升序数列，所以直接和前一个节点对比就可以了，上面的解法也是在数组升序的情况下直接对比，否则还要自己排序，索性直接遍历的时候就对比，时间复杂度是O(n)。

  ```
    int prev = -1;
    int min = Integer.MAX_VALUE;

    public int minDiffInBST(TreeNode root) {

        search(root);
        return min;
    }


    private void search(TreeNode root) {

        if (root == null) {
            return;
        }
        // 搜索到最左边
        search(root.left);
        if (prev != -1) {
            min = Math.min(min, root.val - prev);
        }
        // 从最左边开始，将左节点设为前置节点，到最后
        prev = root.val;

        search(root.right);

    }
  ```