## Greatest Common Divisor of Strings

For strings S and T, we say "T divides S" if and only if S = T + ... + T  (T concatenated with itself 1 or more times)

Return the largest string X such that X divides str1 and X divides str2.

 
- Example 1:

  Input: str1 = "ABCABC", str2 = "ABC"

  Output: "ABC"

- Example 2:

  Input: str1 = "ABABAB", str2 = "ABAB"

  Output: "AB"

- Example 3:

  Input: str1 = "LEET", str2 = "CODE"

  Output: ""
 

- Note:

  - 1 <= str1.length <= 1000
  - 1 <= str2.length <= 1000
  - str1[i] and str2[i] are English uppercase letters.

---

## 解题思路

这道题是说要找出两个字符串中的最大公因子，比如是T，字符串每次除以这个T，就是在字符串中去掉这个T，最后都能除尽。

- 最大公因子

  因子是一个数除以它可以除尽，最大因子数则是最一个数可以除尽的最大的数，最大公因子数表示两个数都可以除尽的最大的数。
  
  - 求24和60的最大公约数
  
    先分解质因数，得24=2×2×2×3，60=2×2×3×5，24与60的全部公有的质因数是2、2、3，它们的积是2×2×3=12，所以，(24，60)=12。 

  - 求ABABAB和ABAB的最大公约数

    先分解质因数，得ABABAB=AB×AB×AB，ABAB=AB×AB，ABABAB+ABAB等于ABAB+ABABAB，所以(ABABAB，ABAB)=AB。 
  
- 辗转相除法

  两个数，求大值对小值的余数，然后循环到最后一个数被除尽，因为字符串是有可能没有最大公因数的，也就是相等但是不相同的时候，需要判断。

  ```
	public static String gcdOfStrings(String str1, String str2) {

		// 两个字符串相等但是不一样，表示没有连续最大公因子
		if (str1.length() == str2.length()) {
			if (!str1.equals(str2)) {
				return "";
			}
		}

		// 将两个字符串放入数组中
		String[] arr = new String[2];
		arr[0] = str1;
		arr[1] = str2;

		// 只要有一个字符串不是空就继续循环
		while (!arr[0].equals("") && !arr[1].equals("")) {

			// 两个字符串相等但是不一样，表示没有连续最大公因子
			if (arr[0].length() == arr[1].length()) {
				if (!str1.equals(str2)) {
					return "";
				}
			}

			// 将长的字符串放索引0，短的放索引1
			String[] strs = sorted(arr[0], arr[1]);
			String longStr = strs[0];
			String shortStr = strs[1];

			// 如果长字符串包含短字符串截取掉，再继续上述步骤
			strs[0] = longStr.replace(shortStr, "");
			strs[1] = shortStr;
			arr = strs;
		}

		// 有字符串替换为空了表示除完了
		return arr[0].equals("") ? arr[1] : arr[0];
	}

	public static String[] sorted(String str1, String str2) {
		String[] arr = new String[2];
		if (str1.length() > str2.length()) {
			arr[0] = str1;
			arr[1] = str2;
			return arr;
		}
		arr[1] = str1;
		arr[0] = str2;
		return arr;
	}
  ```

- 辗转相除法(优化)

  配合自带的API方法，加上代码的判断优化，性能更好。

  ```
	public static String gcdOfStrings1(String str1, String str2) {

		// 两个字符串相等
		if (str1.length() == str2.length()) {
			// 但是不一样，表示没有连续最大公因子
			if (!str1.equals(str2)) {
				return "";
			} 
			// 一样，表示最大公因子就是本身
			else {
				return str1;
			}
		}

		// 始终把常字符串放第一个参数，短的放第二个参数
		if (str2.length() > str1.length()) {
			String temp = str1;
			str1 = str2;
			str2 = temp;
		}

		// 如果有相同前缀，把长字符串中的相同部分截取掉，再继续循环
		if (str1.startsWith(str2)) {
			return gcdOfStrings1(str1.substring(str2.length()), str2);
		}

		return "";
	}

  ```