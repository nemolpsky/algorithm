## Reverse String

Write a function that reverses a string. The input string is given as an array of characters char[].

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.

 

- Example 1:

  Input: ["h","e","l","l","o"]

  Output: ["o","l","l","e","h"]

- Example 2:

  Input: ["H","a","n","n","a","h"]

  Output: ["h","a","n","n","a","H"]

---

## 解题思路

这个题目的意思是颠倒一个字符数组，但是要求空间复杂度必须是O(1)。

- 首尾互换

  遍历数组的前一半，和后面的对应的互换位置即可，时间复杂度是O(n)，空间复杂度是O(1)。

  ```
	public static void reverseString(char[] s) {
		int length = s.length;
		// 循环前一半，和后面的对应的地方互换，如果是奇数长度刚好省略掉中间的
		for (int i = 0; i < length / 2; i++) {
			char temp = s[i];
			// { 'h', 'e', 'l', 'l', 'o' }
			// i=0，5-0-1=4
			// i=1，5-1-1=3
			s[i] = s[length - i - 1];
			s[length - i - 1] = temp;
		}
	}
  ```