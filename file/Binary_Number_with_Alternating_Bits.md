## Binary Number with Alternating Bits

Given a positive integer, check whether it has alternating bits: namely, if two adjacent bits will always have different values.

- Example 1:

  Input: 5

  Output: True

  Explanation:

  The binary representation of 5 is: 101

- Example 2:
  
  Input: 7

  Output: False

  Explanation:

  The binary representation of 7 is: 111.

- Example 3:
  
  Input: 11

  Output: False

  Explanation:

  The binary representation of 11 is: 1011.

- Example 4:
  
  Input: 10

  Output: True

  Explanation:

  The binary representation of 10 is: 1010.

---

## 解题思路

这道题是说给定一个数字，需要转化为二进制数，然后看它的所有为数是不是01010101这样交错排列的，如果有连续的就返回false，否则返回true。

- 求余计算

  直接手动计算每个位数的数字，然后同上一个位数对比，每次都不一样就表示是交错排列的，时间复杂度是O(n)。

  ```
	public static boolean hasAlternatingBits(int n) {

		// 保存上一个位数
		int temp = -1;
		while (n != 0) {
			// 计算位数
			int i = n % 2;
			// 相同表示不是交错的
			if (temp == i) {
				return false;
			}
			// 将当前值保存
			temp = i;

			// 计算下位数
			n /= 2;
		}
		return true;
	}
  ```

  