## Middle of the Linked List

Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

 

- Example 1:

  Input: [1,2,3,4,5]

  Output: Node 3 from this list (Serialization: [3,4,5])

  The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).

  Note that we returned a ListNode object ans, 
  
  such that:

  ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.

- Example 2:

  Input: [1,2,3,4,5,6]

  Output: Node 4 from this list (Serialization: [4,5,6])
  Since the list has two middle nodes with values 3 and 4, we return the second one.
 

- Note:

  - The number of nodes in the given list will be between 1 and 100.

---

## 解题思路

这道题是一个链表，需要返回中间那个节点，如果长度是偶数，中间节点会有2个就返回第二个，长度是1的时候返回第一个，是2则返回第二个。

- map

  遍历链表，按顺序存储到map中，再根据计算出的中点值取，时间复杂度是O(n)。
  
  ```
	public static ListNode middleNode(ListNode head) {
		ListNode result = null;

		// 保存所有节点，key是顺序
		HashMap<Integer, ListNode> map = new HashMap<>();

		// 遍历链表存储到map中
		int index = 0;
		ListNode node = head;
		while (node.next != null) {
			map.put(index, node);
			node = node.next;
			index++;
		}

		// 判断下长度为1、2的情况
		if (index + 1 == 1) {
			return head;
		}

		if (index + 1 == 2) {
			return head.next;
		}

		// 计算出中点，从map中取
		int middle = index + 1;
		result = map.get(middle / 2 + 1);

		return result;
	}
  ```