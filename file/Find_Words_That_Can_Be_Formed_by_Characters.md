## Find Words That Can Be Formed by Characters

You are given an array of strings words and a string chars.

A string is good if it can be formed by characters from chars (each character can only be used once).

Return the sum of lengths of all good strings in words.


- Example 1:

  Input: words = ["cat","bt","hat","tree"], chars = "atach"

  Output: 6

  Explanation: 

  The strings that can be formed are "cat" and "hat" so the answer is 3 + 3 = 6.

- Example 2:

  Input: words = ["hello","world","leetcode"], chars = "welldonehoneyr"
  
  Output: 10

  Explanation: 

  The strings that can be formed are "hello" and "world" so the answer is 5 + 5 = 10.

- Note:
 
  - 1 <= words.length <= 1000
  - 1 <= words[i].length, chars.length <= 100
  - All strings contain lowercase English letters only.
---

## 解题思路

这道题目的意思是给定一个字符串数组，里面都是由小写字母组成的单词。再给定一个字符串，需要判断这个数组里有多少个字符串可以由这个特定字符串里的字母组合起来，注意每次判断的时候特定字符串里的字母只能使用一次，最后要算出所有能组合起来的单词加起来的字母总数。

- 遍历加map

  首先遍历数组再遍历字符串是必须的操作，然后将特定字符串里所有字母出现次数都保存在一个Map中，这样每次取操作都是O(1)。然后对比每个单词，每次都新建一个Map来保存被对比的单词的字母出现次数，只要有一个字母的出现次数比特定单词中的字母出现的次数大就表示无法组成这个字母就可以排除。时间复杂度是O(n)。

  ```
    public int countCharacters(String[] words, String chars) {
        int count = 0;

        // 将特定字符串每个字符存入map，vale是个数
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        for (char c : chars.toCharArray()) {
            int value = map.get(c) == null ? 0 : map.get(c);
            map.put(c, value + 1);
        }

        // 遍历数组
        for (String str : words) {
            int length = str.length();
            // 建立一个map存放该单词中每个字母的出现次数
            HashMap<Character, Integer> countMap = new HashMap<Character, Integer>();
            // 遍历单词每个字母
            for (char c : str.toCharArray()) {
                Integer value = map.get(c);
                int innerValue = countMap.get(c) == null ? 0 : countMap.get(c);
                // 如果查不到该字母或者出现次数超过了特定字符串中的次数就表示该单词无法组成，直接中断循环
                if (value != null && innerValue < value) {
                    countMap.put(c, innerValue + 1);
                } else {
                    length = 0;
                    break;
                }
            }
            count += length;
        }

        return count;
    }
  ```

- 遍历加数组

  这个方法其实原理是和数组的一模一样，只不过很巧妙的利用了ASCII值，a的ASCII值是97，后面的字母都是连续的，所以所有字母先减去97，这样字母的ASCII值就是0-25，索引就是key，value就是出现次数。

  ```
    public static int countCharacters(String[] words, String chars) {
        // 存放26个字母的数组
        int[] charsArr = new int[26];
        // 遍历存放，索引就是ASCII值，value就是次数
        for (char c : chars.toCharArray()) {
            // 减去'a'是为了数组索引从0开始
            int asciiValue = c - 'a';
            charsArr[asciiValue]++;
        }

        int count = 0;

        // 遍历判断
        for (String word : words) {
            if (isGoodString(word, charsArr)) {
                count += word.length();
            }
        }
        return count;
    }

    public static boolean isGoodString(String word, int[] charsArr) {
        // 新建一个数组用于存放被判断的单词的字母出现次数
        int[] arr = new int[26];
        boolean result = true;
        for (char c : word.toCharArray()) {
            // 索引是减去了a，所以判断的时候也要减去
            int value = c - 'a';
            // 如果没查到就直接中断排除该单词，查到了就保存下当前字母的出现次数
            if (charsArr[value] > 0) {
                arr[value]++;
            } else {
                result = false;
                break;
            }

            // 如果出现的次数大于特定单词的字母出现次数也一样失败
            if (arr[value]>charsArr[value]){
                result = false;
            }
        }
        return result;
    }
  ```