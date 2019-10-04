## Array Partition I

Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

- Example 1:
  
  Input: [1,4,3,2]

  Output: 4

  Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).

- Note:
  - n is a positive integer, which is in the range of [1, 10000].
  - All the integers in the array will be in the range of [-10000, 10000].

---

## 解题思路

这道题是说给定一个数组，无序，长度在1-10000之间，一定会是偶数，值在-10000-10000之间，会有重复的值，首先把数组排序，然后每两个数配成一对，(n1,n2)(n3,n4)这样的配对，然后找出每对数字里面最小的数字，如果相同则返回任意一个，求所有的最小值的和。

- 排序再累加

  这里直接用的自带的Arrays.sort()方法，时间复杂度是O(nlog(n))，然后因为排序之后，是从小到大，既然是(n1,n2)(n3,n4)的配对方式，所以遍历的时候直接取奇数位置即可，或者直接搁一位数来相加遍历，时间复杂度就是1/2n。

  ```
    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);

        int count = 0;
        for (int i = 0; i < nums.length; ) {
            count += nums[i];
            i = i + 2;
        }

        return count;
    }
  ```
