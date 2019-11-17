## Contains Duplicate

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

- Example 1:

  Input: [1,2,3,1]

  Output: true

- Example 2:

  Input: [1,2,3,4]

  Output: false

- Example 3:

  Input: [1,1,1,3,3,4,3,2,4,2]

  Output: true

---

## 解题思路

这道题是说判断一个数组中有没有一个值出现超过两次，可能会有多个数超过两次。

- 排序

  排序之后所有一样的数字都会挨在一起，遍历判断每个数字即可，时间复杂度是O(nlogn)。

  ```
	public static boolean containsDuplicate(int[] nums) {

		// 快速排序
		Arrays.sort(nums);

		// 因为排序的缘故，所有一样的值会挨在一起，判断下一个数值是否一样即可
		for (int i = 0; i < nums.length - 1; i++) {
			if (nums[i] == nums[i + 1]) {
				return true;
			}
		}
		return false;
	}
  ```