## Find the Difference

Given two strings s and t which consist of only lowercase letters.

String t is generated by random shuffling string s and then add one more letter at a random position.

Find the letter that was added in t.

- Example:

  Input:
  
  s = "abcd"
  t = "abcde" 

  Output:
  
  e
  

  Explanation:

  'e' is the letter that was added.

---

## 解题思路

这道题的意思是给定一个字符串s，然后再给定一个字符串t，t是在s的基础上添加了一个多余的字符，然后乱序重排，需要找出这个多余的字符。

- 数组保存位置

  思路也很清晰，先保存s的所有字母出现次数，然后再对比t删减次数，最后剩下的就是多余的那个字母，时间复杂度是O(n)。

  ```
	public static char findTheDifference(String s, String t) {
		int[] arr = new int[26];

		// 存储出现的字符
		for (char c : s.toCharArray()) {
			arr[c - 'a']++;
		}

		// 对比减去存储的字符
		for (char c : t.toCharArray()) {
			arr[c - 'a']--;
		}

		// 找到剩下唯一的那个字符
		char c = 'a';
		int index = 0;
		for (int i : arr) {
			if (i != 0) {
				c = (char) (index + 'a');
			}
			index++;
		}
		return c;
	}

  ```