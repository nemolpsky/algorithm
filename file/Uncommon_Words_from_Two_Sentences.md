## Uncommon Words from Two Sentences

We are given two sentences A and B.  (A sentence is a string of space separated words.  Each word consists only of lowercase letters.)

A word is uncommon if it appears exactly once in one of the sentences, and does not appear in the other sentence.

Return a list of all uncommon words. 

You may return the list in any order.

 

- Example 1:

  Input: A = "this apple is sweet", B = "this apple is sour"

  Output: ["sweet","sour"]

- Example 2:

  Input: A = "apple apple", B = "banana"

  Output: ["banana"]
 
- Note:

  - 0 <= A.length <= 200
  - 0 <= B.length <= 200
  - A and B both contain only spaces and lowercase letters.

--- 

## 解题思路
这道题是给定两个字符串，分别是两个句子，找出没有在两个句子中都出现的单词。

- HashMap

  首先是要对两个字符串分割后遍历，记录每个单词的出现次数，可以放到一起遍历，最后再查看map中哪个单词只出现了1次，时间复杂度是O(n)。

  ```
	public static String[] uncommonFromSentences(String A, String B) {

		List<String> list = new ArrayList<String>();

		// 存放次数
		HashMap<String, Integer> map = new HashMap<>();

		// A B分割出来的数组和长度
		String[] aArr = A.split(" ");
		String[] bArr = B.split(" ");
		int Alength = aArr.length;
		int Blength = bArr.length;

		// 两个数组一起遍历
		for (int i = 0; i < Math.max(Alength, Blength); i++) {
			if (i < Alength) {
				String a = aArr[i];
				int value = map.getOrDefault(a, 0);
				map.put(a, value + 1);
			}

			if (i < Blength) {
				String b = bArr[i];
				int value = map.getOrDefault(b, 0);
				map.put(b, value + 1);
			}
		}

		// 查找哪个次数等于1
		for (Map.Entry<String, Integer> entry : map.entrySet()) {
			if (entry.getValue() > 1) {
				list.add(entry.getKey());
			}
		}

		return list.toArray(new String[list.size()]);
	}
  ```