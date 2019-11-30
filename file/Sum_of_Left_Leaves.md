## Sum of Left Leaves

Find the sum of all left leaves in a given binary tree.

- Example:

  ```
    3
   / \
  9  20
    /  \
   15   7
  ```

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

---

## 解题思路

这道题是说计算二叉树的所有左叶子节点的累加和。

- 递归

  只要熟悉递归和深度优先遍历这道题还是相当简单的，主要是加一个标识来判断当前节点是否是左节点，再判断是否是叶子节点即可，时间复杂度是O(n)。

  ```
	int sum = 0;

	public int sumOfLeftLeaves(TreeNode root) {
		if (root == null) {
			return 0;
		}

		search(root, false);
		sum -= root.val;
		return sum;
	}


	public void search(TreeNode node, boolean isLeft) {
		
		// 遍历到根节点了返回
		if (node == null) {
			return;
		}

		// 是左节点，并且是个叶子节点
		if (isLeft && node.left == null && node.right == null) {
			System.out.println(sum + ";" + node.val);
			sum += node.val;
		}

		// 递归
		search(node.left, true);
		search(node.right, false);
	}

  ```
