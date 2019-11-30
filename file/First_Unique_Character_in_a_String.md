## First Unique Character in a String

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

- Examples:

  ```
  s = "leetcode"
  return 0.

  s = "loveleetcode",
  return 2.
  ```

- Note: You may assume the string contain only lowercase letters.

---

## 解题思路
这道题的意思是找出字符串中不重复的字符中索引最小的那个字符的索引位置

- 数组保存字符出现次数和位置

  先遍历一次字符串，保存每个字符的出现次数和在字符中的索引，然后遍历存储次数的数组，找出出现次数为1并且索引最靠前的那个字符的索引，时间复杂度是O(n)。

  ```
	public int firstUniqChar(String s) {

		if (s.isEmpty() || s.equals("")) {
			return -1;
		}

		// 存储次数的数组，索引是字母-'a'，value是出现次数
		int[] count = new int[26];
		// 存储位置的数组，索引是字母-'a'，value是字符的索引
		int[] position = new int[26];

		// 遍历存储字符的出现次数和索引位置
		int index = 1;
		for (char c : s.toCharArray()) {
			count[c - 'a']++;
			position[c - 'a'] = index;
			index++;
		}

		int uniqIndex = Integer.MAX_VALUE;
		for (int i = 0; i < count.length; i++) {
			// 遍历所有不重复的字符的索引，找到最小的那个
			if (count[i] == 1) {
				uniqIndex = Math.min(uniqIndex, position[i]);
			}
		}

		// 索引位置还是等于最大值表示没有不重复的字母
		if (uniqIndex == Integer.MAX_VALUE) {
			return -1;
		}

		return uniqIndex - 1;
	}

  ```