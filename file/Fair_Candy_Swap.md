## Fair Candy Swap

Alice and Bob have candy bars of different sizes: A[i] is the size of the i-th bar of candy that Alice has, and B[j] is the size of the j-th bar of candy that Bob has.

Since they are friends, they would like to exchange one candy bar each so that after the exchange, they both have the same total amount of candy.  (The total amount of candy a person has is the sum of the sizes of candy bars they have.)

Return an integer array ans where ans[0] is the size of the candy bar that Alice must exchange, and ans[1] is the size of the candy bar that Bob must exchange.

If there are multiple answers, you may return any one of them.  It is guaranteed an answer exists.

 

- Example 1:

  Input: A = [1,1], B = [2,2]

  Output: [1,2]

- Example 2:

  Input: A = [1,2], B = [2,3]

  Output: [1,2]

- Example 3:

  Input: A = [2], B = [1,3]

  Output: [2,3]

- Example 4:

  Input: A = [1,2,5], B = [2,4]

  Output: [5,4]
 

- Note:

  - 1 <= A.length <= 10000
  - 1 <= B.length <= 10000
  - 1 <= A[i] <= 100000
  - 1 <= B[i] <= 100000
  - It is guaranteed that Alice and Bob have different total amounts of candy.
  - It is guaranteed there exists an answer.
---

## 解题思路
这道题目是说给定两个数组，数组中的值分别是不同糖果的重量，需要找到相互交换一对数字来让两个数组中的糖果总重量相等。

- 数学公式

  这种题目一般都要考虑从数学的思维去解决，否则暴力破解性能会很差，也很难解。用了公式之后直接套公式就会非常简单，因为每次要对比所以需要把数组A的值放到map中，这样每次对比操作的时间复杂度是O(1)，整个时间复杂度是O(n)，n等于最小的数组的长度。
  ```
  1.假设SA、SB分别是A数组和B数组的长度
  2.如果A、B数组互换一对数字然后结果就相同之后，公式就是SA + x - y = SB + y - x，x是数组B中的，y是数组A中的
  3.最后简化为 y = (SA - SB)/2 + x
  ```

  ```
    public static int[] fairCandySwap(int[] A, int[] B) {
        // SA + x - y = SB + y - x;
        // y = (SA - SB)/2 + x

        // 存储A数组所有元素
        HashMap<Integer,Integer> position = new HashMap<>();
        int sumA = 0;
        // 计算A数组和
        for (int i : A) {
            position.putIfAbsent(i,i);
            sumA += i;
        }

        // 计算B数组和
        int sumB = 0;
        for (int i : B) {
            sumB += i;
        }
        int sum = (sumB - sumA) / 2;

        int[] arr = new int[2];
        // 遍历B数组对比A数组
        for (int i : B) {
                // 如果y值减去(SA - SB)/2的值存在就表示找到结果了
                Integer value = position.get(i - sum);
                if (value != null) {
                    arr[0] = value;
                    arr[1] = i;
                    break;
                }
        }

        return arr;
    }
  ```
