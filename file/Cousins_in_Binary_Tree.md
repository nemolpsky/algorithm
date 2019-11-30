## Cousins in Binary Tree

In a binary tree, the root node is at depth 0, and children of each depth k node are at depth k+1.

Two nodes of a binary tree are cousins if they have the same depth, but have different parents.

We are given the root of a binary tree with unique values, and the values x and y of two different nodes in the tree.

Return true if and only if the nodes corresponding to the values x and y are cousins.

 

- Example 1:


  Input: root = [1,2,3,4], x = 4, y = 3

  Output: false

- Example 2:


  Input: root = [1,2,3,null,4,null,5], x = 5, y = 4

  Output: true

- Example 3:



  Input: root = [1,2,3,null,4], x = 2, y = 3

  Output: false
 

- Note:

  - The number of nodes in the tree will be between 2 and 100.
  - Each node has a unique integer value from 1 to 100.

---

## 解题思路

这道题目是给定一棵二叉树，再给定两个值，需要判断这两个值是否既满足了不是同一个父节点又在同一深度的条件。

- 深度优先搜索

  实现思路肯定是深度优先搜索遍历找到这两个值所在节点的深度，对比是否一样，同是是否是同一个父节点，这里使用一个map来存储，key是父节点，value是深度，更加方便操作，时间复杂度是O(n)。

  ```
	public boolean isCousins(TreeNode root, int x, int y) {

		HashMap<Integer, Integer> map = new HashMap<>();
		search(root, 0, x, y, map, 0);

		// 因为是父节点做key所以如果是相同的父节点map只会有一个值
		if (map.size() <= 1) {
			return false;
		}

		// 对比两个值的层级是否一样
		int index = 1;
		int first = -1;
		int second = -1;
		for (Integer i : map.values()) {
			if (index == 1) {
				first = i;
			} else {
				second = i;
			}
			index++;
		}

		return first == second ? true : false;
	}

	public void search(TreeNode root, int level, int x, int y, HashMap<Integer, Integer> map, int value) {
		// 数组超过两个就表示已经找到两个值了不需要继续遍历
		if (map.size() >= 2) {
			return;
		}

		if (root == null) {
			return;
		}

		// 找到值就存到map中
		if (root.val == x || root.val == y) {
			map.put(value, level);
		}

		// 递归深度优先搜索
		search(root.left, level + 1, x, y, map, root.val);
		search(root.right, level + 1, x, y, map, root.val);
	}

  ```