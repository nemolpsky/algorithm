## Prime Number of Set Bits in Binary Representation

Given two integers L and R, find the count of numbers in the range [L, R] (inclusive) having a prime number of set bits in their binary representation.

(Recall that the number of set bits an integer has is the number of 1s present when written in binary. For example, 21 written in binary is 10101 which has 3 set bits. Also, 1 is not a prime.)

- Example 1:

  Input: L = 6, R = 10

  Output: 4

  Explanation:

  ```
  6 -> 110 (2 set bits, 2 is prime)
  7 -> 111 (3 set bits, 3 is prime)
  9 -> 1001 (2 set bits , 2 is prime)
  10->1010 (2 set bits , 2 is prime)
  ```

- Example 2:

  Input: L = 10, R = 15

  Output: 5

  Explanation:

  ```
  10 -> 1010 (2 set bits, 2 is prime)
  11 -> 1011 (3 set bits, 3 is prime)
  12 -> 1100 (2 set bits, 2 is prime)
  13 -> 1101 (3 set bits, 3 is prime)
  14 -> 1110 (3 set bits, 3 is prime)
  15 -> 1111 (4 set bits, 4 is not prime)
  ```
- Note:

  - L, R will be integers L <= R in the range [1, 10^6].
  - R - L will be at most 10000.

---

## 解题思路

这道题的意思是给定一个两个数字L和R，需要依次判断这两个数字[L,R]之间的所有数字的二进制数，获取1的个数，然后看个数值是不是质数，最后要统计总共多少个质数。

- 转换二进制遍历

  这种办法是效率最低的，3分钟内就能想出来，转成二进制字符串，判断有多少个1，然后判断个数是不是质数。

  ```
    public static int countPrimeSetBits(int L, int R) {
        int count = 0;

        for (int i = L; i <= R; i++) {
            String binary = Integer.toBinaryString(i);
            int sum = 0;
            for(String s:binary.split("")){
                if (s.equals("1")){
                    sum++;
                }
            }
            if (isPrime(sum)){
                count++;
            }
        }

        return count;
    }

    public static boolean isPrime(int a) {

        boolean flag = true;

        if (a < 2) {// 素数不小于2
            return false;
        } else {

            for (int i = 2; i <= Math.sqrt(a); i++) {

                if (a % i == 0) {// 若能被整除，则说明不是素数，返回false

                    flag = false;
                    break;// 跳出循环
                }
            }
        }
        return flag;
    }
  ```

- 限制优化

  首先因为数值范围限定在了[1,10^6]，十进制最大是10^6，换成二进制，就是2^20，因此所有质数一定在20内，所以直接写死就好。另外Integer方法提供了获取二进制1个数的方法，底层是一套比较复杂的算法，效率要比上面的快很多，有时候自己是在手写不出来，知道用也不失为一个好办法，时间复杂复杂度是O(nlogn)。

  ```
    public static int countPrimeSetBits(int L, int R) {
        int count = 0;

        for (int i = L; i <= R; i++) {
            // 获取十进制数字的二进制数上所有的1
            int sum = Integer.bitCount(i);
            if (isPrime(sum)) {
                count++;
            }
        }

        return count;
    }

    // 因为最多不会超过判断19
    public static boolean isPrime(int x) {
        return (x == 2 || x == 3 || x == 5 || x == 7 ||
                x == 11 || x == 13 || x == 17 || x == 19);
    }
  ```