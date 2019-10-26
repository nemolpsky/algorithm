## Occurrences After Bigram

Given words first and second, consider occurrences in some text of the form "first second third", where second comes immediately after first, and third comes immediately after second.

For each such occurrence, add "third" to the answer, and return the answer.

 

- Example 1:

  Input: text = "alice is a good girl she is a good student", first = "a", second = "good"

  Output: ["girl","student"]

- Example 2:

  Input: text = "we will we will rock you", first = "we", second = "will"

  Output: ["we","rock"]
 

- Note:

  - 1 <= text.length <= 1000
  - text consists of space separated words, where each word consists of lowercase English letters.
  - 1 <= first.length, second.length <= 10
  - first and second consist of lowercase English letters.

---

## 解题思路
这道题的意思是给定一个句子，里面有很多个单词，都由空格分隔开，提供两个参数，分别是fisrt和second，需要找到句子中所有符合first后跟着second的格式下second后面的单词，然后全部返回。

- 遍历对比每个元素的后续两位

  先分割成数组，然后遍历元素，判断后元素自己和后一个是否符合规则，后两个元素不需要判断，因为连第三个元素都没有了。这个思路上还可以继续优化，不适用自带的spilt方法，自己写一个，在这个遍历过程中直接判断，时间复杂度是O(n)。

  ```
	public static String[] findOcurrences(String text, String first, String second) {

		List<String> list = new ArrayList<>();

		String[] words = text.split(" ");

		for (int i = 0; i < words.length; i++) {
            // 最后两个元素不用判断
			if (i < words.length - 2) {
                // 判断顺序后后一个单词，如果符合就把第三个放入集合
				if (words[i].equals(first) && words[i + 1].equals(second)) {
					list.add(words[i + 2]);
				}
			}
		}

		return list.toArray(new String[list.size()]);
	}
  ```