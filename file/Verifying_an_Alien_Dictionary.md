## Verifying an Alien Dictionary

In an alien language, surprisingly they also use english lowercase letters, but possibly in a different order. The order of the alphabet is some permutation of lowercase letters.

Given a sequence of words written in the alien language, and the order of the alphabet, return true if and only if the given words are sorted lexicographicaly in this alien language.

 

- Example 1:

  Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"

  Output: true

  Explanation: 
  
  As 'h' comes before 'l' in this language, then the sequence is sorted.

- Example 2:

  Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"

  Output: false

  Explanation: 
  
  As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.

- Example 3:

  Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"

  Output: false

  Explanation: 
  
  The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).
 

- Constraints:

  - 1 <= words.length <= 100
  - 1 <= words[i].length <= 20
  - order.length == 26
  - All characters in words[i] and order are English lowercase letters.

---

## 解题思路

这道题本身并不难，就是写的太模糊了，给定一个字符串，是乱序的26个字母，称之为字典表，给定一个数组，判断数组是否按规则排序，规则就是数组中相邻两个单词进行比较，比较的方式是相同索引位置上的字母进行比较，相同索引位置上前一个单词的字母在上面字典表中必须是排在后一个字母索引上的字母前边，如果长度不够的话，视为空字符，是最小的，还有最恶心的一点题目中根本没讲，如果在前面的索引上判断符合排序，后面的索引就都不用判断了，直接视为符合排序。

- 数组存储字典表

  唯一要注意的就是将字典表的顺序记录下来，这样每次对比都是O(1)的时间复杂度，然后就是判断成功后的循环跳出，整体时间复杂度是O(n)。

  ```
	public static boolean isAlienSorted(String[] words, String order) {

		int[] arr = new int[26];

		// 保存字典字母的顺序
		int index = 0;
		for (char c : order.toCharArray()) {
			arr[c - 'a'] = index++;
		}

		// 遍历单词，每个和后面一个对比
		for (int i = 0; i < words.length - 1; i++) {
			String first = words[i];
			String second = words[i + 1];

			int firstLength = first.length();
			int secondLength = second.length();

			System.out.println(first + "-" + second);
			boolean isSorted = false;
			// 遍历单词字符
			for (int l = 0; l < Math.min(firstLength, secondLength); l++) {
				isSorted = false;
				// 两个单词上同一个索引位置的字母
				Character firstChar = first.charAt(l);
				Character secondChar = second.charAt(l);

				// 相同不比较
				if (firstChar == secondChar) {
					continue;
				}

				// 不相同的根据字典表对比
				if (arr[firstChar - 'a'] > arr[secondChar - 'a']) {
					return false;
				} else {
					// 判断排序后面的索引就不需要判断了
					isSorted = true;
					break;
				}
			}

			// 上面的循环已经判断了短字符串上所有的字符，如果走到这步的话表示前面的判断都是符合排序的
			// 而如果前面单词比后面单词长，就表示空字符最小的放后面了，排序就错了
			if (!isSorted && firstLength > secondLength) {
				return false;
			}
		}

		return true;
	}

  ```