## Number of Lines To Write String
We are to write the letters of a given string S, from left to right into lines. Each line has maximum width 100 units, and if writing a letter would cause the width of the line to exceed 100 units, it is written on the next line. We are given an array widths, an array where widths[0] is the width of 'a', widths[1] is the width of 'b', ..., and widths[25] is the width of 'z'.

Now answer two questions: how many lines have at least one character from S, and what is the width used by the last such line? Return your answer as an integer list of length 2.

 

- Example :
  
  Input: 
  
  widths = [10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10],S = "abcdefghijklmnopqrstuvwxyz"

  Output: 
  
  [3, 60]

  Explanation: 
  
  All letters have the same length of 10. To write all 26 letters,we need two full lines and one line with 60 units.

- Example :

  Input: widths = [4,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10],S = "bbbcccdddaaa"

  Output: 
  
  [2, 4]

  Explanation: 

  All letters except 'a' have the same length of 10, and "bbbcccdddaa" will cover 9 * 10 + 2 * 4 = 98 units.For the last 'a', it is written on the second line because there is only 2 units left in the first line.So the answer is 2 lines, plus 4 units in the second line.
 

- Note:

  - The length of S will be in the range [1, 1000].
  - S will only contain lowercase letters.
  - widths is an array of length 26.
  - widths[i] will be in the range of [2, 10].

---

## 解题思路
这道题的意思是给定一个数组，里面标明了26个字母所占用的字符长度，然后再给定一个字符串，假设将字符串里面所有的字符写下来，每一行是100长度，如果长度不够就换行，最后输出占用的行数和最后一行的长度，例如[2,10]就表示占用两行，最后一行占用长度10。

- 遍历计算

  对每个字母都做减'a'的操作，这样可以获取到该字母在存储字母长度数组中对应的索引，然后获取长度，初始化一行的长度为100，每次遍历都减去字符长度，如果不够就换行，最后计算下最后一行的长度就可以了，时间复杂度是O(n)。

  ```
	public static int[] numberOfLines(int[] widths, String S) {
		// 写入的行数
		int count = 1;
		// 最后一行的数量
		int last = 0;
		// 每行宽度
		int length = 100;

		// 遍历字符串
		for (int i = 0; i < S.length(); i++) {
			// 该字符所占长度
			int cLength = widths[S.charAt(i) - 'a'];

			// 如果最后的位置放不下直接放到下一行
			if (length < cLength) {
				// 到新的一行重置各种变量
				count++;
				length = 100;

			}
			// 每行长度减去字符长度
			length -= cLength;

		}

		// 如果一行的长度不等于100就表示有未填满的最后一行
		if (length != 100) {
			last = 100 - length;
		}

		return new int[] { count, last };
	}
  ```