## N-ary Tree Level Order Traversal

Given an n-ary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example, given a 3-ary tree:

![tree](https://github.com/nemolpsky/algorithm/raw/master/file/image/tree2.png) 

We should return its level order traversal:

```
[
     [1],
     [3,2,4],
     [5,6]
]
```

- Note:

  - The depth of the tree is at most 1000. 
  - The total number of nodes is at most 5000.
  
---

## 解题思路

这道题的意思是将一棵二叉树同一层的所有节点值都放入同一个集合中，再把每层的集合放入一个总的集合中。

- 队列

  又是深度优先搜索法，只不过将每层都放到一起操作，换一层就重置一下各种集合和变量，时间复杂度是O(n)。

  ```
	public List<List<Integer>> levelOrder(Node root) {
		// 存放每层数值的集合
		List<List<Integer>> list = new ArrayList<>();

		// 队列
		LinkedList<Node> linkedList = new LinkedList<>();
		linkedList.add(root);

		if (root == null) {
			return list;
		}

		// 添加根节点
		list.add(new ArrayList<>(Arrays.asList(new Integer[] { root.val })));

		// 存放同一层的数值
		List<Integer> currentList = new ArrayList<>();
		// 存放同一层子节点
		LinkedList<Node> childList = new LinkedList<>();
		while (!linkedList.isEmpty()) {

			// 弹出当前节点
			Node currentNode = linkedList.poll();
			// 当前节点的子节点
			if (currentNode.children != null && !currentNode.children.isEmpty()) {
				for (Node child : currentNode.children) {
					childList.add(child);
					currentList.add(child.val);
				}
			}

			// 当这一层的节点都遍历完了
			if (linkedList.isEmpty()) {
				// 如果存放数值的集合不为空则放入
				if (currentList != null) {
					list.add(currentList);
				}
				// 遍历这一层的子节点的队列
				linkedList = childList;
				// 存放子节点和数值的集合都重新置空
				currentList = new ArrayList<>();
				childList = new LinkedList<>();
			}
		}

		return list;
	}
  ```

- 递归

  递归也是相同的道理，遍历所有节点，只不过是需要加个层级标识来判断是哪一层的节点，存放到哪一层的集合，时间复杂度是O(n)。

  ```
	public List<List<Integer>> levelOrder(Node root) {
		List<List<Integer>> list = new ArrayList<>();
		search(root, 0, list);
		return list;
	}

	public void search(Node node, int depth, List<List<Integer>> list) {
		if (node == null) {
			return;
		}

		// 最开始的时候深度和集合长度都是0，所以每次两个值一样的时候都需要建立一个集合存放子节点的数值
		if (depth == list.size()) {
			list.add(new ArrayList<>());
		}
		// 获取该深度层级所在的集合存放值
		list.get(depth).add(node.val);
		for (Node child : node.children) {
			// 递归调用遍历子节点，深度加1
			search(child, depth + 1, list);
		}
	}
  ```