## 二叉树
二叉树是一种树形结构，一个节点会有最多两个节点，而如果一个节点没有任何子节点，则被称为叶子节点，最顶端被称为根节点，相对其他的数据结构来说，查询效率比较高。

---


### 前序遍历

    前序遍历就是下面这张图的顺序，可以看到是从最左边的叶子开始，然后回溯到上个节点再遍历右节点。

    ![前序](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/pre_tree.png)

    前序遍历可以使用堆栈实现，比如下面的这棵二叉树配合这段代码会是下列这样的出入站的顺序，从根节点开始，每次都是按右、左的顺序添加，然后弹出左节点，再循环上面的步骤，这样出栈的顺序就是从最左端的叶子节点开始，一直往上回溯，如果碰到有右节点则会按照同样的顺序遍历，再继续往上回溯，知道全部遍历完，这就是前序遍历。

    ```
              1
            /   \
          4      3
         / \    /  \
       2    6  8    5
      /
     9

     // 入栈和出栈顺序
     [1]          ->   1
     [3,4]        ->   4
     [3,6,2]      ->   2
     [3,6,9]      ->   9
     [3,6]        ->   6
     [3]          ->   3
     [5,8]        ->   8
     [5]          ->   5
    ```

    ```
    public List<Integer> preorderTraversal(TreeNode root) {

        List<Integer> list = new ArrayList<Integer>();

        if (root==null){
            return list;
        }

        Stack<TreeNode> stack = new Stack<TreeNode>();

        stack.add(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            list.add(node.val);

            // 添加右节点
            if (node.right!=null){
                stack.push(node.right);
            }
            // 添加左节点
            if (node.left!=null) {
                stack.push(node.left);
            }
        }

        return list;
    }
    ```

    递归也可以实现，变量的利用了系统的堆栈结构。

    ```
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        if (root == null) {
            return new ArrayList<Integer>();
        }
        // 根节点
        list.add(root.val);
        // 遍历到左边最底部
        if (root.left != null) {
            list.addAll(preorderTraversal(root.left));
        }
        // 遍历到右边最底部
        if (root.right != null) {
            list.addAll(preorderTraversal(root.right));
        }
        return list;
    }
    ```

---

### 中序遍历

    中序遍历是遍历到最左边的叶子节点，然后是父节点，最后是右节点也按这个顺序遍历，一直到所有的节点都遍历完成。

    ![中序](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/in_tree.png)

    中序遍历同样可以使用堆栈实现，可以看到定义了一个节点引用作为当前遍历到的节点，第2个for循环直接到了最左边的叶子节点，然后输出，回溯，如果有右节点，则会按上面的规则来遍历，最后会遍历完所有的节点。

    ```
              1
            /   \
          4      3
         / \    /  \
       2    6  8    5
      /
     9

     // 入栈和出栈顺序
     [1]          
     [1,4]        
     [1,4,2]      
     [1,4,2,9]    ->   9
     [1,4,2]      ->   2
     [1,4]        ->   4
     [1,6]        ->   6
     [1]          ->   1
     [3]
     [3,8]        ->   8
     [3]          ->   3
     [5]          ->   5
    ```

    ```
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        if (root==null){
            return list;
        }

        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode currentNode = root;
        while (currentNode!=null || !stack.isEmpty()) {
            // 添加左节点，直到叶子节点
            while (currentNode!=null) {
                stack.push(currentNode);
                currentNode = currentNode.left;
            }

            // 弹出左边的叶子节点
            currentNode = stack.pop();
            // 输出
            list.add(currentNode.val);
            // 遍历叶子节点
            currentNode = currentNode.right;
        }
        return list;
    }
    ```

    递归实现

    ```
    public List <Integer> inorderTraversal(TreeNode root) {
        List <Integer> res = new ArrayList <> ();
        dfs(root, res);
        return res;
    }

    public void dfs(TreeNode node, List <Integer> res) {
        if (node != null) {
            // 遍历到左边最底部
            if (node.left != null) {
                dfs(node.left, res);
            }
            // 输出根节点
            res.add(node.val);

            // 遍历到右边最底部
            if (node.right != null) {
                dfs(node.right, res);
            }
        }
    }

    ```

---

### 后序遍历

    后序遍历和中序遍历有点类似，但是不同之处在于它是需要先遍历左子树，再遍历右子树，最后遍历根节点，如果右子树下也有子树，则重复这个逻辑。

    ![后序](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/post_tree.png)

    后序遍历也是可以使用堆栈实现的，但是相对于前序遍历和中序遍历来说难度要大一些。因为是先输出左右节点再输出父节点，所以需要定义一个cur变量来判断什么时候到达左叶子节点以及什么时候遍历左节点，同时需要使用last变量保存上次输出的节点，以免遍历到右节点后发生死循环。

    ```
              1
            /   \
          4      3
         / \    /  \
       2    6  8    5
      /
     9

     // 入栈和出栈顺序
     [1]          
     [1,4]        
     [1,4,2]      
     [1,4,2,9]    ->   9  -> last = 9
     [1,4,2]      ->   2  -> last = 2
     [1,4]
     [1,4,6]      ->   6  -> last = 6
     [1,4]        ->   4  -> last = 4
     [1,3]
     [1,3,8]      ->   8  -> last = 8
     [1,3,5]      ->   5  -> last = 5
     [1,3]        ->   3  -> last = 3
     [1]          ->   1  -> last = 1
    ```

    ```
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        if (root == null) {
            return list;
        }

        Stack<TreeNode> stack = new Stack<>();
        // 当前操作的节点
        TreeNode cur = root;
        // 上一个节点
        TreeNode last = null;
        // 当前操作的节点不为空或者栈不为空
        while (cur != null || !stack.isEmpty()) {
            // 当前节点不为空，遍历到它的左子节点
            if (cur != null) {
                stack.push(cur);
                cur = cur.left;
            } 
            // 到达左叶子节点
            else {
                // 获取当前节点
                TreeNode temp = stack.peek();
                // 右子节点不为空且和上一个节点不同，遍历右节点
                if (temp.right != null && temp.right != last) {
                    cur = temp.right;
                } 
                // 否则输出
                else {
                    list.add(temp.val);
                    // 记录为上个节点
                    last = temp;
                    // 出栈
                    stack.pop();
                }
            }
        }
        return list;
    }
    ```

    递归实现

    ```
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        dfs(root, res);
        return res;
    }

    private void dfs(TreeNode node, List<Integer> res) {
        if (node == null) {
            return;
        }
        // 遍历到左边最底部
        dfs(node.left, res);
        // 遍历到右边最底部
        dfs(node.right, res);
        // 输出根节点
        res.add(node.val);
    }
    ```
---

## 层次顺序遍历

   其实上面的前序、中序和后序遍历都是基于堆栈的DFS(深度优先搜索)，同样也可以使用基于队列的BFS(广度优先搜索)，也就是按一层层的顺序去遍历。

   下面就是基于队列的BFS实现，可以看到相比DFS来说BFS还是简单的多，也清晰的多，就是定义一个队列和集合，专门存放每一层的节点，然后一层循环完就重置进行下一层的。

   ```
              1
            /   \
          4      3
         / \    /  \
       2    6  8    5
      /
     9

     [
         [1],
         [4,3],
         [2,6,8,5],
         [9]
     ]
   ```

   ```
    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> list = new ArrayList<>();
        LinkedList<TreeNode> queue = new LinkedList<>();

        if (root == null) {
            return list;
        }

        queue.add(root);

        // 存储每一层的集合
        List<Integer> innerList = new ArrayList<>();
        // 存储每一层的队列
        LinkedList<TreeNode> innerQueue = new LinkedList<>();;
        while (!queue.isEmpty()) {
            // 节点出队放入集合中
            TreeNode currentNode = queue.poll();
            innerList.add(currentNode.val);

            // 左节点放入层队列中
            if (currentNode.left != null) {
                innerQueue.add(currentNode.left);
            }

            // 右节点放入层队列中
            if (currentNode.right != null) {
                innerQueue.add(currentNode.right);
            }

            // 当前队列空了，就遍历下一层的队列并重置层队列，存储每一层数据的集合放入外层集合中并重置。
            if (queue.size() == 0) {
                queue = innerQueue;
                innerQueue = new LinkedList<>();
                list.add(innerList);
                innerList = new ArrayList<>();
            }
        }

        return list;
    }
   ```