## Convert BST to Greater Tree

Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

- Example:

  Input: The root of a Binary Search Tree like this:

  ```
              5
            /   \
           2     13
  ```

  Output: The root of a Greater Tree like this:
  ```
             18
            /   \
          20     13
  ```

---

## 解题思路

这道题是说给定一个二叉平衡树，要求把所有节点在原来的数值基础上再累加上所有数值比它大的值。

- 递归

  递归写还是比较简单的，定义一个值初始为0，然后遍历到右边最深的节点，然后累加这个值，同时也累加节点的值，这样递归最深处往上返回就把每一层的值都累加进来了，刚好二叉平衡树右边都是大值，所以就刚好把节点所有大于它的值都累加进来了，时间复杂度是O(n)。

  ```

    static int num = 0;

	public static TreeNode convertBST(TreeNode root) {
		if (root != null) {
			// 先遍历到最右边的子节点
			convertBST(root.right);

			// 加上遍历过节点的值
			num += root.val;
			// 给当前节点重新赋值
			root.val = num;

			// 再遍历左边节点
			convertBST(root.right);
		}
		return root;
	}

  ```