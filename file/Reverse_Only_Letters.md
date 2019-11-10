## Reverse Only Letters

Given a string S, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.

 

- Example 1:

  Input: "ab-cd"

  Output: "dc-ba"

- Example 2:

  Input: "a-bC-dEf-ghIj"

  Output: "j-Ih-gfE-dCba"

- Example 3:

  Input: "Test1ng-Leet=code-Q!"

  Output: "Qedo1ct-eeLg=ntse-T!"
 

- Note:

  - S.length <= 100
  - 33 <= S[i].ASCIIcode <= 122 
  - S doesn't contain \ or "
---

## 解题思路

这道题目的意思是给定一个字符串，包含数字和符号，ASCII值的范围是在[33,122]之间，需要做的是翻转字符顺序，但是只翻转字母的，其他符号的不变。

- 单独对比字母

  如果不考虑符号，翻转就是原来从头到尾变成了从尾到头，现在加上符号，所以可以先把字母取出来，遍历原字符串，倒着把字母拼起来，如果遇到符号则拼上原来的符号，这样刚好就是只翻转了字母，没动符号，时间复杂度是O(n)。

  ```
    public static String reverseOnlyLetters(String S) {

        // 遍历取出字母字符串
        StringBuilder letters = new StringBuilder();
        for (char c : S.toCharArray()) {
            if (Character.isLetter(c)) {
                letters.append(c);
            }
        }

        StringBuilder builder = new StringBuilder();
        int index = letters.length() - 1;
        // 遍历源字符串，如果是字母就倒着把字母填进来，不是就不动
        for (char c : S.toCharArray()) {
            if (Character.isLetter(c)) {
                builder.append(letters.charAt(index));
                index--;
            } else {
                builder.append(c);
            }
        }

        return builder.toString();
    }
  ```

- 首尾替换

  其实翻转顺序就是左右对应位置相互交换，只需要左右两个索引，找到字母就替换，再进行下一组，时间复杂度是O(n)，n等于S中字母的数量除以2，比上面的要快很多，上面的其实是2n。

  ```
    public static String reverseOnlyLetters1(String S) {

        // 左边索引
        int startIndex = 0;
        // 右边索引
        int endIndex = S.length() - 1;

        char[] arr = S.toCharArray();

        // 左边比右边小，表示还没有遍历完
        while (startIndex < endIndex) {
            
            // 没找到字母且没有索引越界，继续找
            while (!Character.isLetter(arr[startIndex]) && startIndex < S.length() - 1) {
                startIndex++;
            }

            // 没找到字母且没有索引越界，继续找
            while (!Character.isLetter(arr[endIndex]) && endIndex > 0) {
                endIndex--;
            }

            // 判断有没有遍历完，遍历完就不需要再交换了
            if (startIndex < endIndex) {
                // 替换左右
                char temp = arr[startIndex];
                arr[startIndex] = arr[endIndex];
                arr[endIndex] = temp;
                startIndex++;
                endIndex--;
            }
        }

        return String.valueOf(arr);
    }

  ```