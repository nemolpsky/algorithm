## Goat Latin

A sentence S is given, composed of words separated by spaces. Each word consists of lowercase and uppercase letters only.

We would like to convert the sentence to "Goat Latin" (a made-up language similar to Pig Latin.)

The rules of Goat Latin are as follows:

If a word begins with a vowel (a, e, i, o, or u), append "ma" to the end of the word.
For example, the word 'apple' becomes 'applema'.
 
If a word begins with a consonant (i.e. not a vowel), remove the first letter and append it to the end, then add "ma".
For example, the word "goat" becomes "oatgma".
 
Add one letter 'a' to the end of each word per its word index in the sentence, starting with 1.
For example, the first word gets "a" added to the end, the second word gets "aa" added to the end and so on.
Return the final sentence representing the conversion from S to Goat Latin. 

 

- Example 1:

  Input: "I speak Goat Latin"

  Output: "Imaa peaksmaaa oatGmaaaa atinLmaaaaa"

- Example 2:

  Input: "The quick brown fox jumped over the lazy dog"

  Output: "heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"
 

- Notes:

  - S contains only uppercase, lowercase and spaces. Exactly one space between each word. 
  - 1 <= S.length <= 150.
---

## 解题思路

这道题感觉就是一道拼接逻辑题，给定一个句子，按规则修改每个单词(a, e, i, o, or u)，这五个单词开头的单词后面加ma，否则把单词首字母放尾部再加ma，此外每个单词需要加a，第一个加1个a，第二个加2个a，以此类推，返回新句子。

- HashMap加遍历修改

  首先用一个map存储那几个特殊的字母，然后遍历判断，根据规则修改每个字母即可，时间复杂度是O(n)。

  ```
	public static String toGoatLatin(String S) {
		// a, e, i, o, u + ma
		HashMap<Character, Integer> map = new HashMap<>();
		map.put('a', 0);
		map.put('e', 0);
		map.put('i', 0);
		map.put('o', 0);
		map.put('u', 0);
		map.put('A', 0);
		map.put('E', 0);
		map.put('I', 0);
		map.put('O', 0);
		map.put('U', 0);

		// 分割句子
		String[] words = S.split(" ");
		StringBuilder newWord = new StringBuilder();
		int aIndex = 1;
		// 遍历单词
		for (String word : words) {
			// 判断第一个字母
			char c = word.charAt(0);
			// 是元音开头+ma
			if (map.get(c) != null) {
				newWord.append(word + "ma");
				// 循环+a
				int i = aIndex;
				while (i-- > 0) {
					newWord.append("a");
				}
				// 加空格
				newWord.append(" ");
			}
			// 不是需要移到尾部+ma
			else {
				int length = word.length();
				// copy一个新数组加ma,还要把首字母移到最后
				char[] newArr = new char[length + 2];
				System.arraycopy(word.toCharArray(), 1, newArr, 0, length - 1);
				newArr[length - 1] = word.charAt(0);
				newArr[length] = 'm';
				newArr[length + 1] = 'a';
				newWord.append(String.valueOf(newArr));
				int i = aIndex;
				// 循环+a
				while (i-- > 0) {
					newWord.append("a");
				}
				// 加空格
				newWord.append(" ");
			}
			// 加a的数量
			aIndex++;
		}

		// 去掉最后一个空格
		return newWord.deleteCharAt(newWord.length() - 1).toString();
	}
  ```