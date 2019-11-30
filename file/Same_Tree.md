## Same Tree

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

- Example 1:

  Input:   

  ```  
           1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]
  ``` 

  Output: true

- Example 2:

  Input:   
  ```  
           1         1
          /           \
         2             2

        [1,2],     [1,null,2]
  ```

  Output: false

- Example 3:

  Input:     
  ```
           1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]
  ```

  Output: false
---

## 解题思路
这道题目是判断两棵二叉树是否完全一致。

- 递归

  其实这道题目还是比较简单的，就是深度优先搜索，递归的思想数量问题就不大，每次都是遍历两棵树的相同位置的节点，如果有所不同就表示不一样，时间复杂度是O(n)。

  ```
	public boolean isSameTree(TreeNode p, TreeNode q) {

		return false;
	}

	public boolean search(TreeNode node1, TreeNode node2) {

		// 如果遍历到最后两个节点都为空，是一样的返回true
		if (node1 == null && node2 == null) {
			return true;
		}

		// 因为上面判断了全都为空的情况，所以如果有一个为空就表示不一样返回false
		if (node1 == null || node2 == null) {
			return false;
		}

		// 值不一样也返回false
		if (node1.val != node2.val) {
			return false;
		}

		// 每次都是将子节点的对比&&运算之后返回，也就是说如果有一个地方不同就会返回false
		return search(node1.left, node2.left) && search(node1.right, node2.right);
	}

  ```
