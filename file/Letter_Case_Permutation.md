## Letter Case Permutation

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

- Examples:

  Input: S = "a1b2"

  Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

  Input: S = "3z4"

  Output: ["3z4", "3Z4"]

  Input: S = "12345"

  Output: ["12345"]

- Note:

  - S will be a string with length between 1 and 12.
  - S will consist only of letters or digits.
---

## 解题思路

这道题的意思是给定一个字符串，里面有包含数字和字母，需要做的是改动字符串中任意一个字母，找到所有的组合，包括原始的字符串。

- 深度优先搜索

  这道题目的麻烦之处就在于如果多个字母，就会有不同的组合，可以用递归来解决，递归的中止条件就是到达最后一个索引的时候，继续往下，要判断索引上的字符是不是字母，如果是的话需要改变大小写，因为前面的字母需要配合后面的字母进行改变，所以改变完之后还要继续递归，这样就可以在前面字母变的情况下，把后面字母所有的改变的组合也凑齐。比如ab的执行就像下面的步骤，时间复杂度是O(logn)，n是字母的数量，因为组合数是2ⁿ。

  ```
  ab -> 存入，把b变为B
  aB -> 把aB存入，返回
  AB -> a变为A存入，再把B变为b
  aB -> A变为a存入。
  ```

  ```
    public List<String> letterCasePermutation(String S) {

        List<String> list = new ArrayList<>();

        recursive(S.toCharArray(),0,list);

        return list;
    }

    public void recursive(char[] chars, int index, List<String> list) {
        // 中止条件，当索引大于等于最大长度的时候就停止递归
        if (index >= chars.length) {
            list.add(String.valueOf(chars));
            return;
        }

        // 递归代码，一直索引加1，往后调用，直到最后
        recursive(chars, index + 1, list);

        // 上面的代码是直接存储原来的字符串
        // 递归修改字母的大小写再存储一遍
        char c = chars[index];
        // 是大写字母或小写字母
        if (('a' <= c && 'z' >= c) || ('A' <= c && 'Z' >= c)) {
            // 大小写字母互相转换
            if ('a' <= c && 'z' >= c) {
                chars[index] = (char) (c - 32);
            }else{
                chars[index] = (char) (c + 32);
            }
            // 递归到最后一个索引
            recursive(chars,index+1,list);
        }
    }
  ```