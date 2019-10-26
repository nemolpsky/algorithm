## Reverse Words in a String III

Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

- Example 1:
  
  Input: "Let's take LeetCode contest"

  Output: "s'teL ekat edoCteeL tsetnoc"

- Note: 

  - In the string, each word is separated by single space and there will not be any extra space in the string.

---

## 解题思路

这道题的意思是给一个字符串，这个字符串是一个句子，每个单词都是由空格分割，然后需要将每个单词的字母顺序都翻转，但是句子中单词的顺序不变。


- 分割遍历

  最简单的方法，先分割空格，然后倒着遍历单词，重新组成单词，再重新组成句子，时间复杂度是O(n)。

  ```
	public static String reverseWords(String s) {
		String[] arr = s.split(" ");

		StringBuilder builder = new StringBuilder();
		for (int l = 0; l < arr.length; l++) {
			for (int i = arr[l].length() - 1; i >= 0; i--) {
				builder.append(arr[l].charAt(i));
			}
			if (l != arr.length - 1) {
				builder.append(" ");
			}
		}
		return builder.toString();
	}

  ```

- 分割遍历

  如果不想用自带的分割，自己写个分割也可以，时间复杂度也是O(n)。

  ```
	public static String reverseWords(String s) {
		List<String> list = new ArrayList<>();
		StringBuilder builder = new StringBuilder();
		for (int i = 0; i < s.length(); i++) {
			if (s.charAt(i) != 32) {
				builder.append(s.charAt(i));
				if (i + 1 == s.length()) {
					list.add(builder.toString());
				}
			} else {
				list.add(builder.toString());
				builder = new StringBuilder();
			}

		}

		builder = new StringBuilder();
		for (int l = 0; l < list.size(); l++) {
			for (int i = list.get(l).length() - 1; i >= 0; i--) {
				builder.append(list.get(l).charAt(i));
			}
			if (l != list.size() - 1) {
				builder.append(" ");
			}
		}
		return builder.toString();
	}
  ```