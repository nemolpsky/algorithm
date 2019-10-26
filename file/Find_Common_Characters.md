## Find Common Characters

Given an array A of strings made only from lowercase letters, return a list of all characters that show up in all strings within the list (including duplicates).  For example, if a character occurs 3 times in all strings but not 4 times, you need to include that character three times in the final answer.

You may return the answer in any order.

 

- Example 1:

  Input: ["bella","label","roller"]

  Output: ["e","l","l"]

- Example 2:

  Input: ["cool","lock","cook"]

  Output: ["c","o"]
 

- Note:

  - 1 <= A.length <= 100
  - 1 <= A[i].length <= 100
  - A[i][j] is a lowercase letter

---

## 解题思路

这道题目是给定一个数组，里面有多字符组成的字符串，需要找到哪些字符是每个字母都有的，如果每个字符在单词都出现两次那就输出两次。

- 数组保存次数

  既然涉及到了记录次数肯定是要使用map之类的结构，不过数组也可以当天然的map来使用，声明一个数组记录最终次数，初始化一个极大值，然后遍历每个单词，因为查找每个字母都包含的字符可以理解为约分操作，也就是找到分子，所以要找到那个最大的有效分子，因此每次遍历后都对比记录最终次数的数组，如果次数更小，就更新为最小，因为如果一个单词的里的字符出现次数比其他单词出现的次数多就表示不满足其他单词，时间复杂度是O(n²)。

  ```
	public static List<String> commonChars(String[] A) {
		// 存放26个字母的出现次数
		int[] arr = new int[26];
		// 初始化int最大值
		Arrays.fill(arr, Integer.MAX_VALUE);
		for (String str : A) {
			// 记录当前单词所有字母出现次数
			int[] currentArr = new int[26];
			for (char c : str.toCharArray()) {
				currentArr[c - 'a']++;
			}
			// 对比当前记录次数，因为是寻找公共相同字母，也就是取分子，所以取最小的值
			for (int i = 0; i < 26; i++) {
				arr[i] = Math.min(arr[i], currentArr[i]);
			}
		}

		// 遍历数字添加
		List<String> list = new ArrayList<>();
		for (int i = 0; i < arr.length; i++) {
			for (int l = arr[i]; l > 0; l--) {
				list.add(((char) (i + 97)) + "");
			}
		}

		return list;
	}
  ```