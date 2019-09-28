## Remove Outermost Parentheses
A valid parentheses string is either empty (""), "(" + A + ")", or A + B, where A and B are valid parentheses strings, and + represents string concatenation.  For example, "", "()", "(())()", and "(()(()))" are all valid parentheses strings.

A valid parentheses string S is primitive if it is nonempty, and there does not exist a way to split it into S = A+B, with A and B nonempty valid parentheses strings.

Given a valid parentheses string S, consider its primitive decomposition: S = P_1 + P_2 + ... + P_k, where P_i are primitive valid parentheses strings.

Return S after removing the outermost parentheses of every primitive string in the primitive decomposition of S.

 

- Example 1:

  Input: "(()())(())"

  Output: "()()()"

  Explanation: 

  The input string is "(()())(())", with primitive decomposition "(()())" + "(())".
  After removing outer parentheses of each part, this is "()()" + "()" = "()()()".
- Example 2:

  Input: "(()())(())(()(()))"

  Output: "()()()()(())"
  
  Explanation: 

  The input string is "(()())(())(()(()))", with primitive decomposition "(()())" + "(())" + "(()(()))".
  After removing outer parentheses of each part, this is "()()" + "()" + "()(())" = "()()()()(())".

- Example 3:

  Input: "()()"

  Output: ""

  Explanation: 

  The input string is "()()", with primitive decomposition "()" + "()".
  After removing outer parentheses of each part, this is "" + "" = "".
 

- Note:
  - S.length <= 10000
  - S[i] is "(" or ")"
  - S is a valid parentheses string

---

## 解题思路：
本题的意思是接受一个字符串，字符串中会包含多个括号```()```字符，括号中可能是空的，也有可能包含其他字符，甚至是嵌套```()```，但是```()```一定是对称出现的，要求去除最外层的括号，就像上面的三个例子一样

- 区分出最外层的```()```
  
  这道题其实还是有很多解决方法的，但是比较效率高的肯定就是只遍历一次，也必须要遍历一次。首先重点在于区分出多个```()```中哪个是最外层的，所以忽略其他的字符，只判断```()```就好，因为括号都是对称出现的，所以只要计算出最外层括号的```(``` 和 ```)```就可以，因为只要遍历一次，所以时间复杂度是O(n)

  ```
  public static String removeOuterParentheses(String S) {
	
	// 左右括号出现的次数
	int left = 0;
	int right = 0;

	StringBuilder builder = new StringBuilder();
	for (int i = 0; i < S.length(); i++) {

		// 统计左括号的数量
		char c = S.charAt(i);
		if (c == '(') {

			if (left != 0) {
				builder.append(c);
			}

			left += 1;
		} else {// 统计右括号的数量

			right += 1;

			if (left != right) {
				builder.append(c);
			}
		}

		// 左右括号数量一样，表示是一个完成的外层，所有变量清理重新计算
		if (left == right) {
			left = 0;
			right = 0;
		}
	}

	return builder.toString();
  }
  ```

- 优化精简

  上面的代码虽然已经可以实现需求，但是很明显是第一版的结果，代码很冗余，判断很多，其实判断数量的变量只需要有一个就好，例如左括号做自增，右括号做自减，这种归属于代码重构了，但是还是很有必要的

  ```
  public static String removeOuterParentheses(String S) {
      // 用于统计当前左右括号出现数量，出现左括号就加1，右括号就减1
      int count = 0;
	  StringBuilder builder = new StringBuilder();
	  for (int i = 0; i < S.length(); i++) {
		  char c = S.charAt(i);
          // 最外层的左括号一定要去除，值一定是1
		  if (c == '(' && (count += 1) > 1) {
			  builder.append(c);
		  } 
          // 因为括号是左右对称的，因此里层的左右括号+1-1后会抵消，当count等于0就遍历到了最外层的右括号，所以不拼接
          else if (c == ')' && (count -= 1) > 0) {
			  builder.append(c);
		  }
	  }

	  return builder.toString();
  }  
  ```