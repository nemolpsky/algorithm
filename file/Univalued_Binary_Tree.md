## Univalued Binary Tree

A binary tree is univalued if every node in the tree has the same value.

Return true if and only if the given tree is univalued.

 

- Example 1:
  
  ![Example 1](https://github.com/nemolpsky/algorithm/raw/master/file/image/4.png)

  Input: [1,1,1,1,1,null,1]

  Output: true

- Example 2:

  ![Example 2](https://github.com/nemolpsky/algorithm/raw/master/file/image/3.png)

  Input: [2,2,2,5,2]

  Output: false
 

- Note:

  The number of nodes in the given tree will be in the range [1, 100].
  Each node's value will be an integer in the range [0, 99].

---

## 解题思路

这道题思路是给定一个二叉树，这个二叉树会有两种情况，第一种是里面所有的节点中会有一个节点的值跟其他的节点的值不一样，这个时候返回false，还有一种情况是所有节点的值都是一样的，返回true。

- 深度优先搜索

  首先肯定是要遍历整个树，来判断是否有不同的值，只需要依次判断每个节点和它的子节点的值是否相同就可以了。这里可以使用深度优先搜索进行遍历，依次遍历到每条边的最深处，时间复杂度是O(n)。

  ```
    public boolean isUnivalTree(TreeNode root) {
        // 存储节点的队列
		LinkedList<TreeNode> linkedList = new LinkedList<>();

        // 添加根节点
		linkedList.add(root);

        // 循环队列
		while (!linkedList.isEmpty()) {
            // 获取头部元素
			TreeNode currentNode = linkedList.poll();
			if (currentNode != null) {
                // 当前节点的值
				int value = currentNode.val;
                // 判断当前节点的左子节点，如果和当前节点值不一样就直接返回false，否则把左子节点塞到队列尾部
				if (currentNode.left != null) {
					if (!(value == currentNode.left.val)) {
						return false;
					}
                    linkedList.add(currentNode.left);
				}

                // 判断当前节点的右子节点，如果和当前节点值不一样就直接返回false，否则把右子节点塞到队列尾部
				if (currentNode.right != null) {
					if (!(value == currentNode.right.val)) {
						return false;
					}
                    linkedList.add(currentNode.right);
				}
			}
		}

		return true;
	}
  ```

---
## 涉及算法-深度优先算法
用于遍历树或图的算法，大致原理就是先选中一个根节点，然后尽量沿着一边尽量深的进行搜索，搜索完再返回到最近的分支点沿着一边继续深入，如此循环，最后就遍历完所有的节点了，中途如果找到了要寻找的节点就可以停止遍历了。下面这张图就是深度搜索的路径。

![深度搜索的路径](https://github.com/nemolpsky/algorithm/raw/master/file/image/5.png)