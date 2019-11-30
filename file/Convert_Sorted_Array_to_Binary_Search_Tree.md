## Convert Sorted Array to Binary Search Tree


Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

- Example:

  Given the sorted array: [-10,-3,0,5,9],

  One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

  ```
        0
       / \
     -3   9
     /   /
   -10  5
  ```
  ```
        0
       / \
     -10  5
       \   \
       -3   9
  ```

---

## 解题思路

这道题是说给定一个升序排列的数组，需要将它排列成一个高度平衡(所有子节点高度相差不超过1)的二叉搜索树。

- 递归

  这道题因为刚好是升序排列，所以应用了二分查找的原理，每次都找中点，找到最后没法找，中点左边的值就是小值，左分支，右边的值就是大值，右分支，中点就是父节点，时间复杂度是O(n)。

  ```
	public static TreeNode sortedArrayToBST(int[] nums) {
		return sort(nums, 0, nums.length - 1);
	}

	public static TreeNode sort(int[] nums, int left, int right) {
		// 左右指针相等就表示无法继续二分了
		if (left > right) {
			return null;
		}
		// 每次都重新计算中点
		int mid = left + (right - left) / 2;
		// 中点作为根节点
		TreeNode root = new TreeNode(nums[mid]);
		// 设置左节点
		root.left = sort(nums, left, mid - 1);
		// 设置右节点
		root.right = sort(nums, mid + 1, right);
		return root;
	}
  ```