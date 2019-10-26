## Shortest Distance to a Character

Given a string S and a character C, return an array of integers representing the shortest distance from the character C in the string.

- Example 1:

  Input: S = "loveleetcode", C = 'e'

  Output: [3, 2, 1, 0, 1, 0, 0, 1, 2, 2, 1, 0]
 

- Note:

  - S string length is in [1, 10000].
  - C is a single character, and guaranteed to be in string S.
  - All letters in S and C are lowercase.

--- 

## 解题思路
这道题是说给定一个单词，然后再给定一个字符，列出所有位置上字符距离这个特定字符最近的距离。

- 左右两次遍历

  首先思路是需要找出左右最近的特定字符，然后看哪个最近，所以可以先遍历左边，每次都拿左边最近的特定字符位置来计算和当前字符的距离，这样最左边的可能会有一些没有计算到，所以再从右边开始遍历，对比两个距离的大小，顺便也把左边空缺的填满了。

  ```
	public static int[] shortestToChar(String S, char C) {
		int[] arr = new int[S.length()];
		int index = Integer.MIN_VALUE/2;

		// 从头开始遍历，
		for (int i = 0; i < S.length(); i++) {
			// 找到C就保存位置
			if (S.charAt(i) == C) {
				index = i;
			} else {
				// 拿左边的C对比
				// 距离C的位置就是当前索引减去C的索引
				// 因为C可能不是在最开头，这个时候C的位置是负的，相减之后会是一个极大的正值
				arr[i] = i - index;
			}
		}

		index = Integer.MAX_VALUE;
		// 从尾开始遍历
		for (int i = S.length() - 1; i >= 0; i--) {
			// 找到C就保存位置
			if (S.charAt(i) == C) {
				index = i;
			} else {
				// 拿右边的C对比
				// 距离C的位置就是C的索引减去当前索引，然后对比原来存储的距离，看哪个小
				// 因为C可能不是在最右边，所以C的位置是一个极大值
				arr[i] = Math.min(index - i, arr[i]);
			}
		}
		return arr;
	}

  ```