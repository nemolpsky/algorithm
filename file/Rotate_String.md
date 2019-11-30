## Rotate String


We are given two strings, A and B.

A shift on A consists of taking string A and moving the leftmost character to the rightmost position. For example, if A = 'abcde', then it will be 'bcdea' after one shift on A. Return True if and only if A can become B after some number of shifts on A.

- Example 1:

  Input: A = 'abcde', B = 'cdeab'

  Output: true

- Example 2:

  Input: A = 'abcde', B = 'abced'

  Output: false

- Note:

  - A and B will have length at most 100.

---

## 解题思路

这道题是说给定A、B两个字符串，依次将A字符串的首字符移动到最后，判断是否可以通过这种移动将A字符串改变为和B字符串相同。

- 遍历更改对比

  最简单的方法就是每次都修改一下进行对比，就是使用StringBuilder来进行更改，时间复杂度是O(n)。

  ```
	public boolean rotateString(String A, String B) {
		
		if (A.equals(B)) {
			return true;
		}

		int length = A.length();

		// 保存每次移动之后的字符串
		String currentStr = A;

		for (int i = 0; i < length; i++) {
			// 在每次移动之后的基础上继续操作
			StringBuilder builder = new StringBuilder(currentStr);
			// 每次都把第一个字符放到最后，再删除掉第一个字符进行对比
			builder.append("1");
			builder.setCharAt(length, currentStr.charAt(0));
			currentStr = builder.deleteCharAt(0).toString();
			// 两个字符串相同就表示能交换成一样的
			if (currentStr.equals(B) == true) {
				return true;
			}
		}

		return false;
	}
  ```

- 拼接判断包含

  还有个更简单的方法，可以发现如果每次把首字符放到后面，但是却不移除的话，字符串A会变成它自己拼接自己，所以只需要判断拼接后是否包含B字符串即可，时间复杂度是O(1)。

  ```
	public static boolean rotateString(String A, String B) {
		return A.length() == B.length() && (A + A).contains(B);
	}
  ```