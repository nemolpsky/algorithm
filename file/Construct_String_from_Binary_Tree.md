## Construct String from Binary Tree

You need to construct a string consists of parenthesis and integers from a binary tree with the preorder traversing way.

The null node needs to be represented by empty parenthesis pair "()". And you need to omit all the empty parenthesis pairs that don't affect the one-to-one mapping relationship between the string and the original binary tree.

- Example 1:

  Input: Binary tree: [1,2,3,4]
  ```
        1
      /   \
     2     3
    /    
   4 
  ```    

  Output: "1(2(4))(3)"

  Explanation: 
  
  Originallay it needs to be "1(2(4)())(3()())", but you need to omit all the unnecessary empty parenthesis pairs. And it will be "1(2(4))(3)".

- Example 2:

  Input: Binary tree: [1,2,3,null,4]
  ```
       1
     /   \
    2     3
     \  
      4 
  ```

  Output: "1(2()(4))(3)"

  Explanation: 
  
  Almost the same as the first example, except we can't omit the first parenthesis pair to break the one-to-one mapping relationship between the input and the output.

---

## 解题思路

这道题的意思是要求根据特定格式打印二叉树，要求一个节点后面的括号中需要包括它所有的左子节点，如果空则打印空的()。


- 递归

  递归的话其实只要对判断条件非常清晰就不难，时间复杂度是O(n)。

  ```
    public static String tree2str(TreeNode t) {
        // 空节点什么都不输出
        if (t == null) {
            return "";
        }
        // 没有自己子节点只输出自己的值
        if (t.left == null && t.right == null) {
            return t.val + "";
        }
        // 左节点不空右节点空，输出当前节点加上(左节点)
        if (t.right == null) {
            return t.val + "(" + tree2str(t.left) + ")";
        }
        // 左右节点都不为空，输出左右节点
        return t.val + "(" + tree2str(t.left) + ")(" + tree2str(t.right) + ")";
    }
  ```

- 递归StringBuilder优化

  上面是直接用字符串拼接实现的，但是其实底层是每次拼接都新建一个StringBuilder对象来实现拼接再转字符串，如果迭代够深，内存消耗会非常大，所以改成使用一个StringBuilder来拼接，性能会好很多。

  ```
    public String tree2str(TreeNode t) {
        StringBuilder builder = new StringBuilder();
        append(t,builder);
        return builder.toString();
    }

    public void append(TreeNode t,StringBuilder builder) {
        // 空节点什么都不输出
        if (t == null) {
            return ;
        }
        // 输出自己的值
        builder.append(t.val);

        // 左节点不空，输出当前节点(左节点)
        if (t.left != null) {
            builder.append("(");
            append(t.left,builder);
            builder.append(")");
        }
        // 左右节点都不为空，输出左右节点
        if (t.right != null) {
            if (t.left == null) {
                builder.append("()");
            }
            builder.append("(");
            append(t.right,builder);
            builder.append(")");
        }
    }

  ```