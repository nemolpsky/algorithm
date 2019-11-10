## Delete Node in a Linked List

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Given linked list -- head = [4,5,1,9], which looks like following:


![node](https://github.com/nemolpsky/algorithm/raw/master/file/image/delete_node1.png)
 

- Example 1:

  Input: head = [4,5,1,9], node = 5

  Output: [4,1,9]

  Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.

- Example 2:

  Input: head = [4,5,1,9], node = 1

  Output: [4,5,9]

  Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.
 

- Note:

  - The linked list will have at least two elements.
  - All of the nodes' values will be unique.
  - The given node will not be the tail and it will always be a valid node of the linked list.
  - Do not return anything from your function.
---

## 解题思路

这道题又是一道类似脑经急转弯的题目，很容易让人感觉题目是不是有问题，其实就是给定一个单向链表，需要删除一个节点，只给定你这个节点，但是不给你这个节点前的节点，也不给首节点。

- 替换下个节点

  因为移除某个节点肯定需要把它的前节点和后节点相连，但是拿不到前节点，所以可以把当前节点值改成和后节点一样，就有两个后节点，再把第二个节点去掉，就可以了，时间复杂度是O(1)。

  ```
    // 1,2,3,4 ; 2 
    // 1,3,3,4 ; 重复的节点3
    // 1,3,4   ; 删掉第二个节点3
    public static void deleteNode(ListNode node) {

        // 将当前节点的值改为下一个节点的值，相当于有两个重复的节点
        node.val = node.next.val;
        // 删除重复的节点
        node.next = node.next.next;

    }
  ```