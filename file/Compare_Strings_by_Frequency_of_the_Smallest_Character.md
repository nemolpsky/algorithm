## 

Let's define a function f(s) over a non-empty string s, which calculates the frequency of the smallest character in s. For example, if s = "dcce" then f(s) = 2 because the smallest character is "c" and its frequency is 2.

Now, given string arrays queries and words, return an integer array answer, where each answer[i] is the number of words such that f(queries[i]) < f(W), where W is a word in words.

 

- Example 1:

  Input: queries = ["cbd"], words = ["zaaaz"]

  Output: [1]

  Explanation: On the first query we have f("cbd") = 1, f("zaaaz") = 3 so f("cbd") < f("zaaaz").

- Example 2:

  Input: queries = ["bbb","cc"], words = ["a","aa","aaa","aaaa"]

  Output: [1,2]

  Explanation: On the first query only f("bbb") < f("aaaa"). On the second query both f("aaa") and f("aaaa") are both > f("cc").
 

- Constraints:

  - 1 <= queries.length <= 2000
  - 1 <= words.length <= 2000
  - 1 <= queries[i].length, words[i].length <= 10
  - queries[i][j], words[i][j] are English lowercase letters.

---

## 解题思路
这道题是的意思是给定两个数组，一个是queries，另一个是words，每个数组里面都是字符串，需要做的是找到每个字符串中最小的那个字母的字数，然后拿queries比较words中所有位置上的这个数字，看queries每个索引上的数字在words中有多少个比这个数字大，将大于这个数字的数量存到集合中。

- HashMap

  先抽取了一个时间复杂度O(n)的计算最小字母次数的方法，然后保存两个数组的数字，最后双层遍历对比，时间复杂度是O(n²)。

  ```
	public static int f(String word) {
		int count = 0;
		char small = 'z';
		// 从最大的开始比较,如果有比自己的小就替换,如果相同就自增数量
		for (char c : word.toCharArray()) {
			if (c < small) {
				small = c;
				count = 1;
			} else if (c == small) {
				count++;
			}
		}
		return count;
	}
   
	public static int[] numSmallerByFrequency(String[] queries, String[] words) {
		// 存储两个数组中的数字的map
		HashMap<Integer, Integer> qMap = new HashMap<>();
		HashMap<Integer, Integer> wMap = new HashMap<>();

		int qLength = queries.length;
		int wLength = words.length;

		// 存入map
		for (int i = 0; i < Math.max(qLength, wLength); i++) {
			if (i < qLength) {
				qMap.put(i, f(queries[i]));
			}

			if (i < wLength) {
				wMap.put(i, f(words[i]));
			}
		}

		int index = 0;
		// 双层遍历对比
		int[] arr = new int[qMap.values().size()];
		for (int i : qMap.values()) {
			int size = 0;
			for (int l : wMap.values()) {
				if (l > i) {
					size++;
				}
			}
			arr[index] = size;
			index++;
		}

		return arr;
	}

  ```