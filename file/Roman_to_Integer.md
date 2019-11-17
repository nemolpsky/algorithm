## Roman to Integer

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

- Example 1:

  Input: "III"

  Output: 3

- Example 2:

  Input: "IV"

  Output: 4

- Example 3:

  Input: "IX"

  Output: 9

- Example 4:

  Input: "LVIII"

  Output: 58

  Explanation: L = 50, V= 5, III = 3.

- Example 5:

  Input: "MCMXCIV"

  Output: 1994

  Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

---

## 解题思路

这道题就是要求按照规则把罗马数字转换成阿拉伯数字，就是一道逻辑判断题。

- map

  转换的规则非常简单，就是有6个倒置的规则刚好相反，规则加起来也不过十来个，可以用map保存起来，直接遍历计算即可，主要是每次都遍历两个元素对比，如果这两个加起来并不符合倒置的特殊规则那就只加第一个元素，再拿下一个元素和它的下一个元素对比，如果符合这两个元素就都算使用过了，时间复杂度是O(n)。

  ```
    public static int romanToInt(String s) {
        HashMap<String, Integer> map = new HashMap<>();

        map.put("I", 1);
        map.put("V", 5);
        map.put("X", 10);
        map.put("L", 50);
        map.put("C", 100);
        map.put("D", 500);
        map.put("M", 1000);


        map.put("IV", 4);
        map.put("IX", 9);
        map.put("XL", 40);
        map.put("XC", 90);
        map.put("CD", 400);
        map.put("CM", 900);

        int sum = 0;
        for (int i = 0; i <= s.length() - 1;) {
            if (i == s.length() - 1) {
                Character last = s.charAt(i);
                sum += map.get(last.toString());
                break;
            }

            Character first = s.charAt(i);
            Character second = s.charAt(i + 1);
            //先两个判断
            Integer value = map.get(first.toString() + second.toString());
            if (value != null) {
                sum += value;
                // 两个元素都不用再判断
                i += 2;
            }
            //两个组合不是特别组合就分别相加
            else {
                sum += map.get(first.toString());
                // 第二个元素要重新判断
                i++;
            }
        }
        return sum;
    }
  ```

- 直接if-else判断

  既然已经知道对比规则，其实改成嵌套的if-else来的更直观，因为倒置的规则都是比正常的规则要小，那么判断加上第二个元素符合就减去多余的值就好，时间复杂度也是O(n)，而且没有上面代码的字符串的各种转换，性能更好。

  ```
    public static int romanToInt(String s) {
        // 字符串长度
        int length = s.length() - 1;
        int sum = 0;
        // 遍历字符串
        while (length >= 0) {
            char c = s.charAt(length);
            // 先判断当前元素，先叠加
            if (c == 'I') {
                sum += 1;
            } else if (c == 'V') {
                sum += 5;
                // 如果第二个元素可以跟第一个元素组成倒置的规则再减去多余的值
                if (length > 0 && s.charAt(length - 1) == 'I') {
                    sum -= 1;
                    length--;
                }
            } else if (c == 'X') {
                sum += 10;
                if (length > 0 && s.charAt(length - 1) == 'I') {
                    sum -= 1;
                    length--;
                }
            } else if (c == 'L') {
                sum += 50;
                if (length > 0 && s.charAt(length - 1) == 'X') {
                    sum -= 10;
                    length--;
                }
            } else if (c == 'C') {
                sum += 100;
                if (length > 0 && s.charAt(length - 1) == 'X') {
                    sum -= 10;
                    length--;
                }
            } else if (c == 'D') {
                sum += 500;
                if (length > 0 && s.charAt(length - 1) == 'C') {
                    sum -= 100;
                    length--;
                }
            } else if (c == 'M') {
                sum += 1000;
                if (length > 0 && s.charAt(length - 1) == 'C') {
                    sum -= 100;
                    length--;
                }
            }
            length--;
        }
        return sum;
    }
  ```