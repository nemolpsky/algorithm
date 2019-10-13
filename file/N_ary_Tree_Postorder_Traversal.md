## N-ary Tree Postorder Traversal

Given an n-ary tree, return the postorder traversal of its nodes' values.

For example, given a 3-ary tree:

![树表](https://github.com/nemolpsky/algorithm/raw/master/file/image/1.png)
 

Return its postorder traversal as: [5,6,3,2,4,1].

 
- Note:

  Recursive solution is trivial, could you do it iteratively?

---

## 解题思路

这道题目和上一道题几乎一模一样，就是输出值的顺序要求不同，在上道题目的基础上修改下排序即可。
- 迭代

  ```
    public List<Integer> postorder(Node root) {
        List<Integer> list = new ArrayList<Integer>();
    	
    	LinkedList<Node> stack = new LinkedList();
    	
    	stack.push(root);
    	
    	while(!stack.isEmpty()) {
    		Node topNode = stack.pop();
    		if (topNode!=null) {
				list.add(topNode.val);
			
				List<Node> children  = topNode.children;
				if (children!=null) {
					for(int i=0;i<=children.size()-1;i++) {
						stack.push(children.get(i));
					}
				}
    		}
    	}
    	
    	Collections.reverse(list);
    	
    	return list;
    }
  ```