
## Two Sum IV - Input is a BST

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

- Example 1:

  Input: 
  ```
      5
     / \
    3   6
   / \   \
  2   4   7
  ```

  Target = 9

  Output: True
 

- Example 2:

  Input: 
  ```
      5
     / \
    3   6
   / \   \
  2   4   7
  ```
  
  Target = 28

  Output: False

---

## 解题思路

这道题是说给定一颗二叉搜索树，再给定一个目标数值，要判断树中是否有两个值相加等于这个目标数值。

- HashSet

  首先如果想要判断一个数据集里面有没有两个值相加等于一个值，就是遍历每个值，拿目标值减去这个值，看看在这个数据集里有没有求出的差值，所以先来一次深度优先搜索把所有的值放到Set中，这样最后再遍历集合判断差值是否在集合中，每次判断都是O(1)的时间复杂度，总体时间复杂度是O(n)。
  
  ```
	public static boolean findTarget(TreeNode root, int k) {

		if (root == null) {
			return false;
		}

		HashSet<Integer> set = new HashSet<>();
		// 保存每个节点的值
		saveValue(root, set);

		// 遍历对比，如果目标值减去一个元素得出结果可以在set中
		// 找到就表示有两个数可以相加等于目标值，但是这两个值不能一样
		for (int i : set) {
			int value = k - i;
			if (value == i) {
				return false;
			}
			if (set.contains(value)) {
				return true;
			}
		}

		return false;
	}

	public static void saveValue(TreeNode node, HashSet<Integer> set) {
		if (node != null) {

			// 当前节点的值
			set.add(node.val);

			// 左节点
			saveValue(node.left, set);

			// 右节点
			saveValue(node.right, set);
		}
	}
  ```

- HashSet优化 

  其实发现可以在深度优先搜索到每个节点的时候就去判断set中是否包含差值，因为是树结构，即使第一个数判断的时候第二个数还没放入set中，判断第二个数的时候也一定把第一个数放进去了，时间复杂度是O(n)。

  ```
	public static boolean findTarget(TreeNode root, int k) {

		if (root == null) {
			return false;
		}

		HashSet<Integer> set = new HashSet<>();

        深度优先搜索
		return search(root, set, k);
	}

	public static boolean search(TreeNode node, HashSet<Integer> set, int targetValue) {
		if (node == null) {
			return false;
		}

        // 判断当前set中的数据
		int value = targetValue - node.val;
		if (value != node.val && set.contains(value)) {
			return true;
		}

		set.add(node.val);

        // 左右节点，有一个成功就算找到了
		return search(node.left, set, targetValue) || search(node.right, set, targetValue);

	}

  ```