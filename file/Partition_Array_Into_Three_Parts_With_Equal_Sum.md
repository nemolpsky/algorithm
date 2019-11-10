## Partition Array Into Three Parts With Equal Sum

Given an array A of integers, return true if and only if we can partition the array into three non-empty parts with equal sums.

Formally, we can partition the array if we can find indexes i+1 < j with (A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1])

 

- Example 1:

  Input: [0,2,1,-6,6,-7,9,1,2,0,1]

  Output: true

  Explanation: 0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1

- Example 2:

  Input: [0,2,1,-6,6,7,9,-1,2,0,1]

  Output: false

- Example 3:

  Input: [3,3,6,5,-2,2,5,1,-9,4]

  Output: true

  Explanation: 3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
 

- Note:

  - 3 <= A.length <= 50000
  - -10000 <= A[i] <= 10000
---

## 解题思路
这道题需要仔细理解题意，说是给定一个数组，需要判断这个数组是否能够刚好被分成3个不同的数组，且值都相同，最重要的是三个数组的起始索引都是必须连在一起的。

- 求三分之一平均数

  首先分成三段，累计和必须一样，可以算出每段累计和都是整个数组之和的3分之1，再加上表明了三段索引必须是连续的，这就简单了，直接一次遍历就好了，如果等于平均值了就表示找到一段，需要计算下一段了，再判断下是不是三段数组即可，时间复杂度是O(n)。

  ```
    public static boolean canThreePartsEqualSum(int[] A) {
        // 求和
        int sum = 0;
        for (int i : A) {
            sum += i;
        }

        // 三分之一
        int average = sum / 3;

        // 统计有多少个数组
        int num = 0;
        int count = 0;
        // 遍历相加
        for (int i : A) {
            count += i;
            // 如果这一段子项刚好等于平均数就表示要重新计算下一段了
            if (count == average) {
                count = 0;
                num++;
            }
        }

        if (num != 3) {
            return false;
        }
        return true;
    }
  ```