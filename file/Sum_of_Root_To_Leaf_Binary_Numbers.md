## Sum of Root To Leaf Binary Numbers

Given a binary tree, each node has value 0 or 1.  Each root-to-leaf path represents a binary number starting with the most significant bit.  For example, if the path is 0 -> 1 -> 1 -> 0 -> 1, then this could represent 01101 in binary, which is 13.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf.

Return the sum of these numbers.

 

- Example 1:

  ![岛屿](https://github.com/nemolpsky/algorithm/raw/master/file/image/leaf_binary.png)

  Input: [1,0,1,0,1,0,1]

  Output: 22

  Explanation: (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
 

- Note:

  - The number of nodes in the tree is between 1 and 1000.
  - node.val is 0 or 1.
  - The answer will not exceed 2^31 - 1.

---

## 解题思路

这道题的意思是说给定一棵二叉树，每个节点的值不是0就是1，从根节点到达每个最底层节点的数值凑成一个二进制数，需要把所有到达根节点的二进制数转换成十进制数再相加求和。

- 深度优先搜索

  这种其实就是遍历深度优先搜索，将每个数字拼成一个二进制数字，然后在到达根节点后再计算十进制值，然后累加，只要递归用的熟练就很好做，时间复杂度是O(n)。

  ```
	public static int sumRootToLeaf(TreeNode root) {
		return cal(root, "", 0);
	}

	public static int cal(TreeNode root, String v, int count) {

		if (root == null) {
			return 0;
		}

		// 加自己
		v += root.val;

		// 没有子节点了，转成十进制值相加
		if (root.left == null && root.right == null) {
			int value = Integer.valueOf(v, 2);
			return count += value;
		}

		// 遍历到左节点
		if (root.left != null) {
			count = cal(root.left, v, count);
		}

		// 遍历到右节点
		if (root.right != null) {
			count = cal(root.right, v, count);
		}

		return count;
	}

  ```