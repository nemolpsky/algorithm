## Number Complement

Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.

- Note:
  
  The given integer is guaranteed to fit within the range of a 32-bit signed integer.
  You could assume no leading zero bit in the integer’s binary representation.

- Example 1:
  
  Input: 5

  Output: 2

  Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.

- Example 2:
  
  Input: 1

  Output: 0

  Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.

---

## 解题思路

这道题的意思是给定一个十进制的数字，然后转换为二进制，取得它的补位，也就是取反，再获得的这个新的二进制数转换为十进制数。

- 遍历转换

  这个办法是最简单也是最笨的，直接使用自带的API转成二进制字符串，再获取补数后转成十进制。

  ```
    public static int findComplement(int num) {

        // 转为二进制
        String binaryString = Integer.toBinaryString(num);

        // 取反
        StringBuilder builder = new StringBuilder();
        for (String s : binaryString.split("")) {
            builder.append(Integer.valueOf(s) ^ 1);
        }

        // 转回十进制
        return Integer.valueOf(builder.toString(), 2);

    }
  ```