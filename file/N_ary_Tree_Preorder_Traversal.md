## N-ary Tree Preorder Traversal

Given an n-ary tree, return the preorder traversal of its nodes' values.

For example, given a 3-ary tree:

![树表](https://github.com/nemolpsky/algorithm/raw/master/file/image/1.png)
 

Return its preorder traversal as: [1,3,5,6,2,4].

- Node Class
  ```
  class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val,List<Node> _children) {
        val = _val;
        children = _children;
    }
  };
  ``` 

- Note:

  - Recursive solution is trivial, could you do it iteratively?

---

## 解题思路

这道题就是说要把一个树的所有节点的值从左到右，从上到下放入List中返回，而且题目还特别说明了，如果使用递归会非常简单，希望使用迭代。

- 递归

  递归就很简单了，前面写了那么多道关于树结构的题，无非就是一直递归到最低端的节点，增加一些判空处理罢了。

  ```
    public List<Integer> preorder(Node root) {
    	return recursive(root,new ArrayList<Integer>());
    }
    
    public List<Integer> recursive(Node root,List<Integer> list){
    	if (root!=null) {
			list.add(root.val);
		}
    	
    	if (!root.children.isEmpty()) {
			for(Node node:root.children) {
				recursive(node, list);
			}
		}
    	return list;
    }
  ```

- 迭代

  迭代就麻烦一点，需要借助队列来实现，将根节点放入队列中，然后开始执行迭代，每弹出一个节点就把它的子节点也放入队列中，如果队列中还有元素就表示没有迭代完，就是要保持输出的顺序会稍稍麻烦点。

  ```
    public List<Integer> preorder(Node root) {
    	List<Integer> list = new ArrayList<Integer>();
    	
    	LinkedList<Node> stack = new LinkedList();
    	
    	stack.push(root);
    	
    	while(!stack.isEmpty()) {
    		Node topNode = stack.pop();
    		if (topNode!=null) {
				list.add(topNode.val);
			
				List<Node> children  = topNode.children;
				if (children!=null) {
					for(int i=0;i<children.size();i++) {
						stack.push(children.get(i));
					}
				}
    		}
    	}
    	
    	return list;
    }
  ```