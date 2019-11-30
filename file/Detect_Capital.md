## Detect Capital

Given a word, you need to judge whether the usage of capitals in it is right or not.

We define the usage of capitals in a word to be right when one of the following cases holds:

All letters in this word are capitals, like "USA".
All letters in this word are not capitals, like "leetcode".
Only the first letter in this word is capital, like "Google".
Otherwise, we define that this word doesn't use capitals in a right way.
 

- Example 1:

  Input: "USA"

  Output: True
 

- Example 2:

  Input: "FlaG"

  Output: False

---

## 解题思路

这道题就是一个逻辑校验题，给定一个单词，如果字母全小写、全大写或是只有首字母大写都算正确，其他一律算错。

- 遍历校验

  这种地方其实并不难，麻烦之处在于一些边际条件要处理，比如再循环里面判断每个字母是否和前面的相同，如果在这个基础上判断第一个字母大写这种特殊的会比较麻烦，技巧在于尽量把边际条件抽出来单独判断，不要和那些通用的判断放在一起，就会好写很多，时间复杂度是O(n)。

  ```
	public static boolean detectCapitalUse(String word) {

		// 一个字母的是否一定对
		if (word.length() == 1) {
			return true;
		}

		// 前两个字母是否大写
		boolean first = Character.isUpperCase(word.charAt(0));
		boolean bofore = Character.isUpperCase(word.charAt(1));

		if (first != bofore && first == false) {
			return false;
		}
		for (int i = 2; i < word.length(); i++) {
			// 当前字母是否大写
			boolean current = Character.isUpperCase(word.charAt(i));
			// 相隔的字母格式不一样就算错
			if (bofore != current) {
				return false;
			}

			bofore = current;
		}
		return true;
	}

  ```