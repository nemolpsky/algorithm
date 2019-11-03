## Maximum Depth of Binary Tree


Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Note: A leaf is a node with no children.

- Example:

  Given binary tree [3,9,20,null,null,15,7],
  
  ```
     3
    / \
   9  20
     /  \
    15   7
  ```

  return its depth = 3.

---

## 解题思路

这道题就是寻找平衡搜索树的最深节点

- 深度优先搜索法

  这个算法以前讲过了，先找到一边最底部，然后返回上一个分支点，继续往下找，如此循环遍历所有的节点，时间复杂度是O(n)。

  ```
	public int maxDepth(TreeNode root) {

		// 递归方法
		return search(root, 0);
	}

	public static int search(TreeNode root, int count) {
		// 节点为空直接返回当前统计的数字
		if (root == null) {
			return count;
		}
		// 统计数字+1
		count++;
		// 深度
		int depth = count;

		// 递归左节点
		if (root.left != null) {
			// 返回深度+1的值
			depth = Math.max(depth, search(root.left, count));
		}

		// 递归右节点
		if (root.right != null) {
			// 返回深度+1的值
			depth = Math.max(depth, search(root.right, count));
		}

		return depth;
	}

  ```