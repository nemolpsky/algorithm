## Count Binary Substrings

Give a string s, count the number of non-empty (contiguous) substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

- Example 1:

  Input: "00110011"

  Output: 6

  Explanation: 
  
  There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".

  Notice that some of these substrings repeat and are counted the number of times they occur.

  Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.

- Example 2:

  Input: "10101"

  Output: 4

  Explanation: 
  
  There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.

- Note:

  - s.length will be between 1 and 50,000.
  - s will only consist of "0" or "1" characters.
---

## 解题思路

这道题就非常有意思了，给定一个全都有1和0组成的乱序字符串，如果1和0各自在左右，并且对称就表示是一个子字符串，例如10/01，1100/0011，需要找出字符串中有多少个子字符串。

- 分组对比

  可以发现配对次数取决与对称的一半长度，另一半再长也没用，而1100/0011里又包含了10/01，所以对称的长度的一半就是子字符串的个数，所以可以先分组，每组里都是连续的1和0的个数，然后每个元素和下一个对比，找出最长的对称长度的一半，也就是最大子字符串的数量，时间复杂度是O(n)。

  ```
    public static int countBinarySubstrings(String s) {
        int length = s.length();
        // 存放分组的数组，最极端情况下是交替排列，每个字符都是一组
        int[] groups = new int[length];
        int groupIndex = 0;

        // 第一组默认是1个
        groups[0] = 1;
        for (int i = 0; i < length - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                groups[groupIndex]++;
            } else {
                groups[++groupIndex]++;
            }
        }

        // 统计配对的次数
        int count = 0;
        // 因为配对是需要1和0对称
        // 比如10/01是一个
        // 1100/0011是两个，因为包含了10/01
        // 所以配对次数取决与对称的一半长度，另一半再长也没用
        // 所以看取最短的长度，也就是能组成的最大的对称子字符串
        for (int i = 0; i < length - 1; i++) {
            if (groups[i] == 0) {
                break;
            }
            count += Math.min(groups[i], groups[i + 1]);
        }

        return count;
    }
  ```

- 分组对比优化

  上面是先分组，再对比每一组，代码是简单多了，但是也多遍历了一遍，其实在遍历的时候每次都拿当前组和前面的组对比就可以了，只要只需要遍历一遍，时间复杂度是O(n)，空间复杂度是O(1)。

  ```
    public static int countBinarySubstrings1(String s) {
        int length = s.length();

        int current = 1;
        int before = 0;

        // 统计配对的次数
        int count = 0;
        for (int i = 0; i < length - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                // 同一组相加
                current++;
            } else {
                // 每次变组都对比该组和前一组的大小
                count += Math.min(current, before);
                // 把当前组放入前一组变量中
                before = current;
                // 重置当前组的变量
                current = 1;
            }

            // 最后一组遍历完了也要进行对比
            if (i == length - 2) {
                count += Math.min(current, before);
            }
        }

        return count;
    }

  ```