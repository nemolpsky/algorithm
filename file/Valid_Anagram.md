## Valid Anagram

Given two strings s and t , write a function to determine if t is an anagram of s.

- Example 1:

  Input: s = "anagram", t = "nagaram"

  Output: true

- Example 2:

  Input: s = "rat", t = "car"

  Output: false

  
- Note:
  
  - You may assume the string contains only lowercase alphabets.

- Follow up:
  
  - What if the inputs contain unicode characters? How would you adapt your solution to such case?
---

## 解题思路

这道题目是说给定两个字符串，这两个字符串有可能是由相同的字母组成的，但是顺序是打乱的，判断这两个字符串组成字母是否一模一样。

- 数组存储字符

  使用数组存储字符串s的所有字母出现个数，然后遍历t字符串对比这个数组，只要有字母没找到就表示不一样，时间复杂度是O(n)，空间复杂度也是O(1)，因为数组长度是固定26，按照大O表示法就是O(1)。

  ```
	public static boolean isAnagram(String s, String t) {

		// 长度都不相等肯定不是变形
		if (s.length() != t.length()) {
			return false;
		}

		// 存储s的字符
		int[] arr = new int[26];
		for (char c : s.toCharArray()) {
			arr[c - 'a']++;
		}

		// 遍历t，对比所有字符是否能找到
		for (char c : t.toCharArray()) {
			// 每次找到都减1，如果数组中存的值等于0表示该字母没找到，不符合要求
			if (arr[c - 'a']-- == 0) {
				return false;
			}
		}

		return true;
	}
  ```

- 兼容unicode字符
 
  使用map来替代数组，因为unicode字符范围特别大，不适合用数组来手动设置大小。