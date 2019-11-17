## Reorder Data in Log Files

You have an array of logs.  Each log is a space delimited string of words.

For each log, the first word in each log is an alphanumeric identifier.  Then, either:

Each word after the identifier will consist only of lowercase letters, or;
Each word after the identifier will consist only of digits.
We will call these two varieties of logs letter-logs and digit-logs.  It is guaranteed that each log has at least one word after its identifier.

Reorder the logs so that all of the letter-logs come before any digit-log.  The letter-logs are ordered lexicographically ignoring identifier, with the identifier used in case of ties.  The digit-logs should be put in their original order.

Return the final order of the logs.

- Example 1:

  Input: logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]

  Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
 

- Constraints:

  - 0 <= logs.length <= 100
  - 3 <= logs[i].length <= 100
  - logs[i] is guaranteed to have an identifier, and a word after the identifier.

---

## 解题思路

这道题也是比较恶心，就是考察排序，给定了一个数字，里面有很多字符串，每个字符串由空格分割开，分割出来的数组，第一个是固定的标识符，之后的字符串可能全是数字或者是字母，要求数字按原来的顺序放到后面，字母则需要按数组第一个之后的字母排序，如果字母相同则对比标识符。

- 快排

  这道题各种排序规则给的非常模糊，但是只要弄清了排序规则就没什么大问题了，使用的快排算法，时间复杂度是O(nlogn)。

  ```
	public static String[] reorderLogFiles(String[] logs) {

		// 字母日志排序
		Arrays.sort(logs, new Comparator<String>() {

			@Override
			public int compare(String o1, String o2) {
				// 截取两个字母串，只截取一个空格的左右的字符
				String[] strs1 = o1.split(" ", 2);
				String[] strs2 = o2.split(" ", 2);
				// 两个都是字母才比较
				boolean isLetter1 = Character.isLetter(strs1[1].charAt(0));
				boolean isLetter2 = Character.isLetter(strs2[1].charAt(0));
				// 两个字母对比
				if (isLetter1 && isLetter2) {
					// 第一个是标识符，对比标识符后面的字母
					int value = strs1[1].compareTo(strs2[1]);
					// 相等再对比标识符
					if (value == 0) {
						value = strs1[0].compareTo(strs2[0]);
					}
					return value;
				}

				// o1是字母往前排
				if (isLetter1) {
					return -1;
				}
				// o1不是字母
				else {
					// o2是字母往后排
					if (isLetter2) {
						return 1;
					}
					// 都不是字母，不同
					return 0;
				}
			}
		});

		return logs;
	}
  ```