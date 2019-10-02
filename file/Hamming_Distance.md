## Hamming Distance

The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers x and y, calculate the Hamming distance.

- Note:

  0 ≤ x, y < 231.

- Example:

  Input: x = 1, y = 4

  Output: 2

- Explanation:

  1   (0 0 0 1)

  4   (0 1 0 0)
       ↑   ↑

  The above arrows point to positions where the corresponding bits are different.

---

## 解题思路

这道题本身并不难的，难点在于涉及到的概念比较多，题目的要求其实就是给定两个数字，然后把这两个数字的所有位数都转化成二进制，为了保持一致长度，短的那个数字会在前面补0。这样就会得到两个等长的字符串，再逐个对比每个相同索引下的数字是否相同，也就是所谓的海明长度。

- 转化二进制，补齐长度，遍历对比每个位数

  这个是弄清楚所有概念，有了思路后的第一版，速度极慢，在LeetCode要14ms，但是这段代码的目的主要是弄懂思路。

  ```
    public int hammingDistance(int x, int y) {
        int distance = 0;
        // 转二进制
        String tempX = Integer.toBinaryString(x);
        String tempY = Integer.toBinaryString(y);

        // 补齐短的长度
        StringBuilder builder = new StringBuilder();
        if (tempX.length()>tempY.length()){
           int extra = tempX.length() - tempY.length();
           while (extra>0){
               builder.append("0");
               extra--;
           }
            builder.append(tempY);
            tempY = builder.toString();
        }else{
            int extra = tempY.length() - tempX.length();
            while (extra>0){
                builder.append("0");
                extra--;
            }
            builder.append(tempX);
            tempX = builder.toString();
        }

        System.out.println(tempX + "-" + tempY);

        // 遍历对比
        for (int i = 0; i < tempX.length(); i++) {
            char a = tempX.charAt(i);
            char b = tempY.charAt(i);
            System.out.println(a + "-" + b);
            if (a != b) {
                distance++;
            }
        }

        return distance;
    }
  ```

- 求余计算二进制数，计算的过程进行对比
  
  下面这段代码的效率就很高了，把转化二进制和对比一次搞定，求二进制其实就是先%2求得当前位数的数，再整除2，循环下去就可以计算出所有的数。其中判断只要有x、y有一个数没有计算完，另一个数就算计算完也回补0。此外还有很多有意思的计算方法，不过就需要多了解语言的运算符了。

  ```
    public int hammingDistance(int x, int y) {
        int distance = 0;
        int lastXbit = 0;
        int lastYbit = 0;

        while (x > 0 || y > 0) {
            lastXbit = x % 2;
            lastYbit = y % 2;
            x /= 2;
            y /= 2;
            builder1.append(lastXbit);
            builder2.append(lastYbit);
            if (lastXbit != lastYbit) {
                distance++;
            }
        }

        return distance;
    }
  ```