## Minimum Absolute Difference in BST


Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.

- Example:

  Input:
  ```
     1
      \
       3
      /
     2
  ```

  Output:
  
  1

  Explanation:

  The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).
 

- Note: 

  - There are at least two nodes in this BST.

---

## 解题思路

这道题是给定一棵二叉搜索树，需要找到这棵树中两个节点之间差绝对值最小的值。

- 中序遍历

  这道题复杂之处就是在于树的结构不好处理，假如想象成一个数组，而且是有序的，那么找出最小绝对值只需要每个元素和附近的元素遍历即可，而刚好如果使用中序递归遍历二叉搜索树的话，顺序刚好是升序的顺序，这样只需要每次遍历到一个节点，就和前面那个节点对比，然后循环找出最小的差值即可，在遍历完左节点之后就为前置节点赋值，时间复杂度是O(n)。

  ```
	int minAbsNum = Integer.MAX_VALUE;
	int value = 0;

	public int getMinimumDifference(TreeNode root) {
		search(root);
		return minAbsNum;
	}

	public void search(TreeNode root) {
		if (root == null) {
			return;
		}
		System.out.println("val-" + root.val + ":" + "pre-" + value);

		// 递归遍历左分支
		search(root.left);

		if (value != 0) {
			minAbsNum = Math.min(Math.abs(value - root.val), minAbsNum);
		}

		// 遍历完左点设置前点的值，也就是自己
		value = root.val;

		// 递归遍历右分支
		search(root.right);
	}
  ```