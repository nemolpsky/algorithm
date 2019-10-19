## Maximum Depth of N-ary Tree

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

- For example, given a 3-ary tree:

  ![树](https://github.com/nemolpsky/algorithm/raw/master/file/image/narytreeexample1.png)
 

  We should return its max depth, which is 3.

 

- Note:

  - The depth of the tree is at most 1000.
  - The total number of nodes is at most 5000.

---

## 解题思路
这道题的意思就是说给定一个树，然后找到这个树的最大深度。

- 递归

  递归的话就很简单了，一层一层的调用下去，其实就是个遍历，会走到最深的那个节点，每遍历一次都将次数+1，返回最大的那个次数。

  ```
    public static int maxDepth(Node root) {

        return search(root,0);
    }

    public static int search(Node root,int count) {
        if (root==null){
            return count;
        }
        count++;
        int depth = count;
        if (root.children!=null&&root.children.size()>0){
            for (Node child:root.children){
                depth = Math.max(depth,search(child,count));
            }
        }
        return depth;
    }
  ```

- 迭代

  使用迭代的做法就稍稍难一点了，其实运用了广度优先搜索算法，从根节点开始筛选邻近节点，然后继续筛选第二层，第三层。所以这里使用了一个队列，保存的是每一层节点，所以每一次队列取元素的操作就需要深度加1。其实按这个原理使用数组或者list来保存每一层元素，然后直接取数组和list的长度也可以得到深度。

  ```
    public static int maxDepth(Node root) {
        // 根节点为0，深度直接返回0
        int count = 0;
        if (root == null) {
            return count;
        }

        //队列
        LinkedList<List<Node>> list = new LinkedList<>();

        // 将根节点放入队列，相当于第一层
        list.add(Arrays.asList(new Node[]{root}));

        //  循环队列
        while (!list.isEmpty()) {

            // 存放下一层的集合
            List<Node> levelList = new ArrayList<>();

            // 获取队列头部元素
            List<Node> currentLevel = list.poll();

            // 将这一层节点的所有子节点都放到一层中
            for (Node node : currentLevel) {
                if (node.children != null)
                    levelList.addAll(node.children);
            }

            // 如果有下一层则放入队列中
            if (levelList.size()>0) {
                list.add(levelList);
            }
            
            // 增加次数
            count++;
        }
        return count;
    }
  ```