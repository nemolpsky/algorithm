## Reverse Linked List

Reverse a singly linked list.

- Example:

  Input: 1->2->3->4->5->NULL

  Output: 5->4->3->2->1->NULL

- Follow up:

  - A linked list can be reversed either iteratively or recursively. Could you implement both?

---

## 解题思路

这道题的意思非常简单，翻转一个链表。

- 递归

  这道题意思看上去很简单，但是实际上想要时间复杂度是O(n)的话还是颇为复杂的，需要要了解如何替换，可以看到下面的注释里整个链表正序和倒叙之后的排列，会发现最关键的一点就是把每个节点的前后节点调换顺序了，每次都把当前节点的下下个节点替换成当前节点，注意的时候这样会造成一个循环，需要切断这个循环，这样就会形成一棵树的形式，以此类推，把所有的都替换掉，时间复杂度是O(n)。

  ```
	// 1 -> 2 -> ... n-2 -> n-1 -> n -> n+1 -> n+2 -> n+3
	// n+3 -> n+2 -> n+1 -> n -> n-1 -> n-2 ... -> 2 -> 1
	// n -> n+1 ==> (n-1)

    // 1,2,3,4,5
	// 4 -> 1,2,3,4,5,4...   || 1,2,3,4 ; 5,4
	// 3 -> 1,2,3,4,3...5    || 1,2,3 ; 5,4,3
	// 2 -> 1,2,3,2...5,4,3  || 1,2 ; 5,4,3,2
	// 2 -> 1,2,1...5,4,3,2  || 1 ; 5,4,3,2,1
	public static ListNode reverseList(ListNode head) {

		// 没有下一个节点或者第一个节点进来就是空的,是尾部节点了变为头结点
		if (head == null || head.next == null) {
			return head;
		}
		
		// 传入第一个节点，再返回尾部的节点
		ListNode node = reverseList(head.next);
		
		// n -> n+1 ==> (n-1)
		// n节点的下一个节点是n+1，要变成n-1
		// 中间的next就是n，head就是n-1，next.next就是n+1
		head.next.next = head;

		// 第一个节点成为了最后一个节点，所以尾部是空的
		head.next = null;
		return node;
	}
  ```

- 队列遍历

  使用队列来写就会清晰很多，时间复杂度也是O(n)。

  ```
  public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
  }
  ```