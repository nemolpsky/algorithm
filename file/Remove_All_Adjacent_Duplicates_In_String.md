## Remove All Adjacent Duplicates In String

Given a string S of lowercase letters, a duplicate removal consists of choosing two adjacent and equal letters, and removing them.

We repeatedly make duplicate removals on S until we no longer can.

Return the final string after all such duplicate removals have been made.  It is guaranteed the answer is unique.

 

- Example 1:

  Input: "abbaca"

  Output: "ca"

  Explanation: 

  For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
 

- Note:

  - 1 <= S.length <= 20000
  - S consists only of English lowercase letters.

---

## 解题思路

这道题的意思是给定一个字符串，需要移除连在一起重复的字符，注意的是移除之后如果还有连在一起重复的也需要移除，比如abbaca移除bb后成为aaca，所以aa也要移除，最后就剩下ca。

- 队列

  使用队列方式可以当做俄罗斯方块那样理解，第一个方块进来，然后上面的方块落下来，如果相同就两块都消失，如果不相同则叠加，也就是主要关注点都是在当前元素和前一个元素是否相同，如果相同两个都移除，后面的元素自然而然和更前面的元素比较，这也就可以解决掉移除一对连续的元素后，又重新出现的连续元素。

  ```
	public static String removeDuplicates(String S) {

		// 队列
		LinkedList<Character> linkedList = new LinkedList<>();

		// 遍历
		for (char c : S.toCharArray()) {
			
			if (linkedList.isEmpty()) {// 添加第一个元素进来
				linkedList.add(c);
			} else if (linkedList.peekLast() == c) {// 判断后进来的元素跟前一个一样就把前一个弹出
				linkedList.pollLast();
			} else {// 和前一个不一样，放入
				linkedList.add(c);
			}
		}

		StringBuilder builder = new StringBuilder();
		while (!linkedList.isEmpty()) {
			builder.append(linkedList.poll());
		}

		return builder.toString();
	}
  ```