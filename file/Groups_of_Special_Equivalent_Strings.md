## Groups of Special-Equivalent Strings

You are given an array A of strings.

Two strings S and T are special-equivalent if after any number of moves, S == T.

A move consists of choosing two indices i and j with i % 2 == j % 2, and swapping S[i] with S[j].

Now, a group of special-equivalent strings from A is a non-empty subset S of A such that any string not in S is not special-equivalent with any string in S.

Return the number of groups of special-equivalent strings from A.

 
- Example 1:

  Input: ["a","b","c","a","c","c"]

  Output: 3

  Explanation: 3 groups ["a","a"], ["b"], ["c","c","c"]

- Example 2:

  Input: ["aa","bb","ab","ba"]

  Output: 4

  Explanation: 4 groups ["aa"], ["bb"], ["ab"], ["ba"]

- Example 3:

  Input: ["abc","acb","bac","bca","cab","cba"]

  Output: 3

  Explanation: 3 groups ["abc","cba"], ["acb","bca"], ["bac","cab"]

- Example 4:

  Input: ["abcd","cdab","adcb","cbad"]

  Output: 1

  Explanation: 1 group ["abcd","cdab","adcb","cbad"]
 

- Note:

  - 1 <= A.length <= 1000
  - 1 <= A[i].length <= 20
  - All A[i] have the same length.
  - All A[i] consist of only lowercase letters.

---

## 解题思路

这道题的意思也是写的比较模糊，说是给定一个数组，里面会有很多不同的单词，如果两个单词符合特定的规则，就可以算这两个单词是相同的，最后需要把所有相同的单词分成一组，返回有多少组的数量。规则就是单词的字符，奇数索引可以与奇数索引互换，反之偶数索引也一样。比如[abcd,cdab]，a和c都是奇数索引，可以互换，b和d都是偶数索引可以互换，因此abcd可以换成cdab。所以这两个单词被认为是一样的，也就是一组，最终分组的数量就是1。

- 数组存储字符ASCII值

  首先如果两个单词的奇数索引上的字母数量是一样，比如都有abcd，那表示只是有可能顺序不一样，是绝对可以交换成一样的，偶数索引也是同理，所以只需要遍历每个字母，存储它们的奇数索引和偶数索引上的每个字符的数量，最后转成字符串存到set中，如果相同就表示同一组，因为set会去重，所以最后set的长度就是分组的数量，时间复杂度是O(n)。

  ```
	public static int numSpecialEquivGroups(String[] A) {
		
		HashSet<String> set = new HashSet<>();

		// 遍历所有单词
		for (String s : A) {
			// 存储所有字符的位置，26个字母就要有26个长度，还要区分奇数偶数所以需要52个
			char[] arr = new char[52];
			for (int i = 0; i < s.length(); i++) {
				// 将奇数索引和偶数索引上的字母次数都存储起来
				arr[s.charAt(i) - 'a' + 26 * (i % 2)]++;
			}
			// 转成字符存储
			set.add(new String(arr));
		}

		return set.size();
	}
  ```