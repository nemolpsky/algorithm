## Leaf-Similar Trees

Consider all the leaves of a binary tree.  From left to right order, the values of those leaves form a leaf value sequence.

![tree](https://github.com/nemolpsky/algorithm/raw/master/file/image/tree1.png)

For example, in the given tree above, the leaf value sequence is (6, 7, 4, 9, 8).

Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar.


- Note:

  - Both of the given trees will have between 1 and 100 nodes.

---

## 解题思路

这个题目的意思是说给定两个二叉树，然后将每棵树最底层的节点从左到右组成一个数列，如果两个数列相同就返回true，否则返回false。

- 深度优先搜索

  最主要的就是要找出最底层的节点，还得是从左到右的顺序，因此最适合使用深度优先搜索，先搜索到最左边的最底层节点，然后返回上一个分支重复这个步骤，最后对比两个数列即可，时间复杂度是O(n)。

  ```
	public boolean leafSimilar(TreeNode root1, TreeNode root2) {
		List<Integer> list1 = new ArrayList<Integer>(100);
		List<Integer> list2 = new ArrayList<Integer>(100);
		list1 = checkTree(root1, list1);
		list2 = checkTree(root2, list2);
		
		return list1.equals(list2);
	}

	public List<Integer> checkTree(TreeNode node, List<Integer> list) {

		// 没有子节点就放入集合中
		if (node.left == null && node.right == null) {
			list.add(node.val);
		}

		// 先搜索左边
		if (node.left != null) {
			checkTree(node.left, list);
		}

		// 再搜索右边
		if (node.right != null) {
			checkTree(node.right, list);
		}

		return list;
	}
  ```