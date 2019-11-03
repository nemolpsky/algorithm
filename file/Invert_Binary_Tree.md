## Invert Binary Tree

Invert a binary tree.

- Example:

  Input:

  ```
       4
     /   \
    2     7
   / \   / \
  1   3 6   9
  ```

  Output:
  ```
       4
     /   \
    7     2
   / \   / \
  9   6 3   1
  ```

- Trivia:

  This problem was inspired by this original tweet by Max Howell:

  Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.
---

## 解题思路
这道题的意思是二叉树翻转，左右节点的值互换。

- 递归

  ```
    public TreeNode invertTree(TreeNode root) {

        if (root == null) {
            return root;
        }

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
  ```