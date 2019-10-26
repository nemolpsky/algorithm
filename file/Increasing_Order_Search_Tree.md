## Increasing Order Search Tree

Given a binary search tree, rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only 1 right child.

- Example 1:
  
  Input: [5,3,6,2,4,null,8,1,null,null,null,7,9]

  ```
         5
       /  \
      3    6
     / \    \
    2   4    8
   /        / \ 
  1        7   9

  ```

  Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
  ```
  1
   \
    2
     \
      3
       \
        4
         \
          5
           \
            6
             \
              7
               \
                8
                 \
                  9  
  ```
- Note:
  
  The number of nodes in the given tree will be between 1 and 100.
  Each node will have a unique integer value from 0 to 1000.

---

## 解题思路
这道题目是要求把一棵平衡搜索书从最小值到最大值，存到一棵新树上，全都存放到右分支节点上。

- 遍历放入集合

  递归遍历，从左至右，因为是平衡二叉树，所以数值是从左到右是从小到大，所以先放左边，再放父节点，再放右边，时间复杂度是O(n)。

  ```
	public TreeNode increasingBST(TreeNode root) {
		// 存放数值的数组
		List<Integer> values = new ArrayList<>();
		add(root, values);

		TreeNode baseNode = new TreeNode(0);
		TreeNode cureentNode = baseNode;
		for (int i : values) {
			TreeNode node = new TreeNode(i);
			cureentNode.right = node;
			cureentNode = node;
		}
		return baseNode.right;
	}

	public void add(TreeNode node, List<Integer> values) {
		if (node == null) {
			return;
		}

		// 先遍历左节点
		add(node.left, values);
		// 放置当前节点值
		values.add(node.val);
		// 遍历有节点
		add(node.right, values);
	}

  ```

- 遍历直接放置

  基于上面的解法可以直接省略掉list，直接遍历的过程放置值，省掉了一遍list循环，时间复杂度也是O(n)。

  ```
	static TreeNode cureentNode = null;

	public static TreeNode increasingBST(TreeNode root) {
		TreeNode baseNode = new TreeNode(0);
		cureentNode = baseNode;
		add(root);
		return baseNode.right;
	}

	public static void add(TreeNode node) {
		// 节点为空直接返回
		if (node == null) {
			return;
		}
 
		// 一直递归左边的值
		add(node.left);
		// 递归完后把左边的值置空，以免下次递归的时候又进到左值的循环
		node.left = null;
		// 存入值到右边
		cureentNode.right = node;
		// 讲当前节点定义为最新的右节点
		cureentNode = node;

		// 递归右节点
		add(node.right);
	}
  ```