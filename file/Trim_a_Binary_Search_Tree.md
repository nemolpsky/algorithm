## Trim a Binary Search Tree

Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in [L, R] (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

- Example 1:

  Input: 
  ```
    1
   / \
  0   2

  L = 1
  R = 2
  ```

  Output: 
  ```
    1
     \
      2
  ```

- Example 2:

  Input: 
  ```
    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3
  ```

  Output: 
  ```
      3
     / 
    2   
   /
  1
  ```

---

## 解题思路
这道题的意思是给定一个平衡搜索树，然后给定L、R两个值，R>=L，需要判断所有的节点是否在这个数值范围内，如果不在则要移除。

- 递归

  一般这种题目都是需要递归实现，但是递归的代码写上去简单，写起来的时候比较绕，像这个题目，因为是平衡搜索树，左边的值都小，右边的值都大，所以只需要分别判断L和R，然后分别去搜索左右两边的节点即可，直到搜索到符合条件的节点重置为当前节点的子节点，时间复杂度是O(n)。

  ```
	public static TreeNode trimBST(TreeNode root, int L, int R) {
		if (root == null) {
			return null;
		}

		// 判断当前节点是否符合边界值
		if (root.val > R) {
			return trimBST(root.right, L, R);
		}
		if (root.val < L) {
			return trimBST(root.left, L, R);
		}

		// 判断该节点下所有的子节点
		// 判断该节点的左节点
		root.left = trimBST(root.left, L, R);
		// 判断该节点的右节点
		root.right = trimBST(root.right, L, R);

		return root;
	}
  ```


