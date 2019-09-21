## Jewels and Stones

You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

- Example 1:

  Input: J = "aA", S = "aAAbbbb"

  Output: 3

- Example 2:

  Input: J = "z", S = "ZZ"

  Output: 0

- Note:

  S and J will consist of letters and have length at most 50.
  The characters in J are distinct.

---

## 解题思路
接受两个字符串，需要分别找出第一个字符串中每个字符在第二字符串中的次数，然后把每个字符的次数累加起来得到一个总次数。

- 两个字符串的嵌套循环

  时间复杂度取决于两个字符串的长度，如果两个都是n，则时间复杂度就是O(n²)

  ```
    public int numJewelsInStones(String J, String S) {
        // 分别获取每个字符串的字符数组
        char[] js = J.toCharArray();
        char[] ss = S.toCharArray();

        int count = 0;

        // 嵌套循环，判断如果有相等的则次数加1
        for(char i:js) {
            for(char l:ss) {
                if (i == l) {
                    count ++ ;
                }
            }
        }
        return count;
    }
  ```