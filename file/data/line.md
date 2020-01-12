## 线性表

比较官方的定义是，零个或多个元素数据的有序数列。说白了就是数据是一个接一个，首尾相连。


---

### 物理存储结构

值得注意是在物理结构(真实存储在内存中)上分为两种情况，一种是顺序存储结构，另一种是链式存储结构。

    - 顺序存储结构
    
      也就是在内存中存储的时候也确确实实是一个挨着一个存储，构成一个线性表。优点是直接按顺序存储就行了，不需要额外的空间保存后继节点的信息，取数据快，缺点是插入和删除都要移动大量元素，比如长度为10的线性表，删除第4个元素，后面6个元素都要往前进一位填上第4个元素空出来的位置。

    - 链式存储结构

      链式存储结构其实就是常说的链表，它每个节点会有额外的空间来存储后继节点的地址，这样存放的时候不需要真的存放在连续的存储空间呢，但是逻辑上确实保持了线性表的结构，而且删除和插入操作只需要修改其中一个节点上后继节点的信息即可，并不需要移动大量元素，但是缺点就是取数据要遍历。

---


### 单链表

    在实际开发中大部分实现都是基于链表，比如队列和栈之类的都是基于单链表，都有自己的后继节点。

    ```
    public class ListNode {
       int val;
       ListNode next;
       ListNode(int x) { val = x; }
    }

    1 -> 2 -> 3 -> 4 -> 5
    ```

    - 翻转链表

      翻转链表是一个很经典的问题，相比与其他数据结构的问题，还是比较绕的。

      ```
      public ListNode reverseList(ListNode head) {
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

      ```
      public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        // 当前节点
        ListNode curr = head;
        while (curr != null) {
            // 获取当前节点的下一个节点
            ListNode nextTemp = curr.next;
            // 把下一个节点改为前置节点
            curr.next = prev;
            // 前置节点改为当前节点
            prev = curr;
            // 遍历到下一个节点
            curr = nextTemp;
        }
        return prev;
      }
      ```
      