## Longest Uncommon Subsequence I

Given a group of two strings, you need to find the longest uncommon subsequence of this group of two strings. The longest uncommon subsequence is defined as the longest subsequence of one of these strings and this subsequence should not be any subsequence of the other strings.

A subsequence is a sequence that can be derived from one sequence by deleting some characters without changing the order of the remaining elements. Trivially, any string is a subsequence of itself and an empty string is a subsequence of any string.

The input will be two strings, and the output needs to be the length of the longest uncommon subsequence. If the longest uncommon subsequence doesn't exist, return -1.

- Example 1:

  Input: "aba", "cdc"

  Output: 3

  Explanation: The longest uncommon subsequence is "aba" (or "cdc"), 

  because "aba" is a subsequence of "aba", but not a subsequence of any other strings in the group of two strings. 

- Note:

  - Both strings' lengths will not exceed 100.
  - Only letters from a ~ z will appear in input strings.
---

## 解题思路
这道题真的是非常非常简单，感觉就是一个脑筋急转弯，而且描述太模糊，歧义太多了，而且因为通过率只有57%左右，很多人都把这道题想复杂了。说给定两个字符串，要保证的是找到一个长度最长，而且不是另一个字符串的子序列(可以不相同，但是顺序必须一样)即可，如果找不到就返回-1。

- 脑经急转弯

  有三种情况
  
  第一种是两个字符串相同并且相等，那么无论怎么删减，被删减的那个字符串都是另一个字符串的子序列。
  第二种情况是两个字符串相同但是不相等，这种情况下，两个字符串都不可能是对方的子序列，任意返回一个长度即可。
  第三种情况是两个字符不相同，这种情况下，长的字符串是绝对不可能是短的字符串的子序列，需要找的就是最长的，那直接返回长字符串长度即可。

  时间复杂度是O(n)，n是长字符串的长度。
  
  ```
	public int findLUSlength(String a, String b) {
		if (a.equals(b))
			return -1;
		return Math.max(a.length(), b.length());
	}
  ```