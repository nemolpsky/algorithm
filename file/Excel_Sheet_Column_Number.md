## Excel Sheet Column Number

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:
``` 
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

- Example 1:

  Input: "A"

  Output: 1

- Example 2:

  Input: "AB"

  Output: 28

- Example 3:

  Input: "ZY"

  Output: 701
---

## 解题思路

这道题目挺有意思的，题目意思是说根据字母所代表的值来计算多个字母的组合值，其实是可以看作是26进制数计算。

- 10进制计算

  为了方便理解，先看个十进制的计算，可以看到从最高位计算，然后每次到下一位的时候就乘以一个10，再加上新的个位数，直到最后全部都算完了。
  
  ```
	// 346
	// 3
	// 3*10 + 4 = 34
	// 34 * 10 + 6 = 346
	public static int tenToNumber(String s) {
		int value = 0;
		for (int i = 0; i < s.length(); i++) {
			int num = Integer.valueOf(s.charAt(i) + "");
			value = value * 10 + num;
		}
		return value;
	}
  ```
- 26进制计算

  有了上面十进制的对比就清晰很多了，不然上来就是直接26进制很容易看晕，时间复杂度是O(n)。

  ```
	// AAA
	// A(1)
	// A(1) * 26 + A(1) = 27
	// AA(27) * 26 + A(1) = 703
	public static int titleToNumber(String s) {
		int value = 0;
		for (int i = 0; i < s.length(); i++) {
			// 将字母转换成代表的数字
			int num = s.charAt(i) - 'A' + 1;
			value = value * 26 + num;
		}
		return value; 
	}
  ```