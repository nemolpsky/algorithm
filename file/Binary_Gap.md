## 


Given a positive integer N, find and return the longest distance between two consecutive 1's in the binary representation of N.

If there aren't two consecutive 1's, return 0.

 

- Example 1:

  Input: 22

  Output: 2

  Explanation: 

  22 in binary is 0b10110.
  In the binary representation of 22, there are three ones, and two consecutive pairs of 1's.
  The first consecutive pair of 1's have distance 2.
  The second consecutive pair of 1's have distance 1.
  The answer is the largest of these two distances, which is 2.

- Example 2:

  Input: 5
  
  Output: 2
  
  Explanation: 
  5 in binary is 0b101.

- Example 3:

  Input: 6

  Output: 1

  Explanation: 
  6 in binary is 0b110.

- Example 4:

  Input: 8

  Output: 0

  Explanation: 
  8 in binary is 0b1000.
  There aren't any consecutive pairs of 1's in the binary representation of 8, so we return 0.

---
 
- Note:

  - 1 <= N <= 10^9

---

## 解题思路
这道题是给定一个数字，需要转换为二进制数，然后找出二进制字符串中两个1之间最远的距离。

- 自带API

  这个办法是使用了自带的API转成二进制字符串，然后找出所有1的位置，再找出最长1的距离。

  ```
    public int binaryGap(int N) {
        // 转成二进制字符串
        String binaryString = Integer.toBinaryString(N);

        // 将二进制字符串上每个位置上的1存入list
        List<Integer> list = new ArrayList<>();
        String[] strs = binaryString.split("");
        for (int i = 0; i < binaryString.length(); i++) {
            if (strs[i].equals("1")) {
                list.add(i);
            }
        }

        // 遍历List来找出最长距离
        int distance = 0;
        for (int i = 0; i < list.size() - 1; i++) {
            distance = Math.max(list.get(i + 1) - list.get(i), distance);
        }

        return distance;
    }
  ```

- 十进制转二进制

  其实用数组存储信息更加效率高，10^6十进制数字的二进制数最多不超过20位，长度20的数组就可以满足要求，然后每次取2的余数再除以2，最后判断数组中所有的1之间的最长的距离，时间复杂度是O(logn)，也就是计算二进制数的次数。

  ```
    public static int binaryGap(int N) {

        // 20满足10^6的二进制数长度
        int[] arr = new int[20];

        int index = 0;
        int l = 0;
        // 转为二进制
        while (N != 0) {
            int value = N % 2;
            // 判断每位上数字是否为1
            if (value == 1) {
                arr[l] = index;
                l++;
            }
            N = N / 2;
            index++;
        }

        // 找出最长距离
        int distance = 0;
        for (int i = 0; i < arr.length - 1; i++) {
            distance = Math.max(arr[i] - arr[i], distance);
        }

        return distance;
    }
  ```