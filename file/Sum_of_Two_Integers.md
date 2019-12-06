## Sum of Two Integers

Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

- Example 1:

  Input: a = 1, b = 2

  Output: 3

- Example 2:

  Input: a = -2, b = 3

  Output: 1

---

## 解题思路

这道题的意思是要求不使用加号和减号来完成数字的加减。

- ^运算符和&运算符

  这道题主要就是考察^和&等运算符的使用，所以首先要了解这两个符号的作用，可以看到其实就是对1和0两个数分作做异或和与操作

  ```
  a = 5 = 0101
  b = 4 = 0100

  a ^ b 如下：

  0 1 0 1
  0 1 0 0
  -------
  0 0 0 1

  a & b 如下：

  0 1 0 1
  0 1 0 0
  -------
  0 1 0 0
  ```

  接下来再看看如何运用这两个运算符计算，可以观察下二进制运算运算，其实可以发现，就是先进行^操作累加每个位数上的值，但是没有计算进位，而&则计算了进位，不过要右移一位，最后两个二进制数值再进行^操作累加。

  ```
  a = 5 = 0101
  b = 4 = 0100

  0 1 0 1
  0 1 0 0
  -------
  1 0 0 1

  拿十进制数做示范
  a = 568,b = 786

  累加值
  5 6 8
  7 8 6
  -------
  2 4 4    

  计算进位
  5 6 8
  7 8 6
  -------
  1 1 1 

  加上进位
    2 4 4
  1 1 1 0
  -------
  1 3 5 4

  ```

  所以只要按这个步骤计算就可以了，虽然断点来看的话可能是觉得两个数值是无规律的整数变化，但是如果每次都打印二进制数来观察，就会发现其实一直在重复这是三步，直到最后不需要进位就表示计算完成了。

  ```
    public static int getSum(int a, int b) {
        System.out.println("base-a-" + Integer.toBinaryString(a));
        System.out.println("base-b-" + Integer.toBinaryString(b));

        System.out.println("============================");

        // 不需要进位
        while(b != 0){
            // ^算出二进制累加后的结果，但是没有计算进位
            int temp = a ^ b;
            // &是算出进位的结果，<<是把二进制数整体往右进一位
            b = (a & b) << 1;
            a = temp;

            System.out.println("temp-" + Integer.toBinaryString(temp));
            System.out.println("b-" + Integer.toBinaryString(b));
            System.out.println("a-" + Integer.toBinaryString(a));
            System.out.println("============================");

        }
        return a;
    }
  ```