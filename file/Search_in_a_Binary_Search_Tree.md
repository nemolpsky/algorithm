## Search in a Binary Search Tree

Given the root node of a binary search tree (BST) and a value. You need to find the node in the BST that the node's value equals the given value. Return the subtree rooted with that node. If such node doesn't exist, you should return NULL.

For example, 

Given the tree:
```
        4
       / \
      2   7
     / \
    1   3
```

And the value to search: 2
You should return this subtree:
```
      2     
     / \   
    1   3
```
In the example above, if we want to search the value 5, since there is no node with value 5, we should return NULL.

- Note 
  
  that an empty tree is represented by NULL, therefore you would see the expected output (serialized tree format) as [], not null.

---

## 解题思路：
这道题目的意思是给定一个二叉搜索树，根据指定值返回该值对应的节点和下面所有的子节点。

- 遍历搜索

  如果已经做过另一道二叉搜索树求和的题，那这道题会非常简单。本质就是递归遍历循环判断，因为所有节点的值都比右边大比左边小，所以只要判断目标值是比当前节点的值大还是小，就可以判断是要遍历左节点还是右节点。时间复杂度是O(n)。

  ```
    public TreeNode searchBST(TreeNode root, int val) {
        if (root==null){
            return null;
        }

        int currentVal = root.val;
        if (currentVal == val){
            return root;
        }

        if (currentVal>val){
            return searchBST(root.left,val);
        }else {
            return searchBST(root.right,val);
        }
    }
  ```