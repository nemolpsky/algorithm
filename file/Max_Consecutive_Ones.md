## Max Consecutive Ones

Given a binary array, find the maximum number of consecutive 1s in this array.

- Example 1:

  Input: [1,1,0,1,1,1]

  Output: 3

  Explanation: 
  
  The first two digits or the last three digits are consecutive 1s.
    The maximum number of consecutive 1s is 3.

- Note:

  - The input array will only contain 0 and 1.
  - The length of input array is a positive integer and will not exceed 10,000

---

## 解题思路

这道题意思就是说给定一个只有0和1组成的数组，判断数组中连续1的最长的长度。

- 遍历

  解法非常简单，遍历下去，遇到1就相加长度，没遇到就表示要重新开始计算长度了，时间复杂度是O(n)。

  ```
	public static int findMaxConsecutiveOnes(int[] nums) {

		// 最大值
		int maxCount = 0;
		// 当前值
		int count = 0;
		// 遍历
		for (int i : nums) {
			// 等于1长度加1
			if (i == 1) {
				count++;
			} 
			// 不等于，表示连续断掉了，重头开始计算，同时对比该段长度和上一段长度
			else {
				maxCount = Math.max(maxCount, count);
				count = 0;
			}
		}

		// 对比最后一段长度
		maxCount = Math.max(maxCount, count);

		return maxCount;
	}
  ```