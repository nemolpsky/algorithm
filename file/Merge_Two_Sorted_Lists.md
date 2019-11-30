## Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

- Example:

  Input: 
  
  1->2->4, 1->3->4

  Output: 
  
  1->1->2->3->4->4

---

## 解题思路

这道题就是合并两个链表，按每个节点的值升序合并。

- 递归

  主要还是考验对递归调用的时候最小步骤的拆分，就是每次递归对比节点，然后将剩下后面的链表继续对比，时间复杂度是O(n)。

  ```
	public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
		// 链表2为空直接返回链表1
		if (l1 == null) {
			return l2;
		}

		// 链表1为空直接返回链表2
		if (l2 == null) {
			return l1;
		}

		// 对比两个链表的头结点的值，较小的那个节点就是较大的节点的下一个节点
		// 再排除掉作为下一个节点的那个节点递归调用，这样就逐个对比每个节点大小并建立关联了
		if (l1.val < l2.val) {
			l1.next = mergeTwoLists(l1, l2.next);
			return l1;
		} else {
			l2.next = mergeTwoLists(l1.next, l2);
			return l2;
		}
	}
  ```