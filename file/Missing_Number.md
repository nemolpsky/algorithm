## Missing Number

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

- Example 1:

  Input: [3,0,1]

  Output: 2

- Example 2:

  Input: [9,6,4,2,3,5,7,0,1]

  Output: 8

- Note:

  - Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

---

## 解题思路

这道题目就非常有意思，是说给定一个乱序的数组，数组中的数值刚好是一个数列随机缺了一个值，需要找出这个值，要求时间复杂度是O(n)，空间复杂度是O(1)。

- ^异或运算符

  首先^运算符是将两个数值的二进制数字逐位进行对比，相同的返回0，不相同的返回1，这也就是说如果两个数如果相同最后会返回0，而数组中刚好缺了一个值，索引也直到了n-1，所以索引这边再加一个n值，这样两两配对就会有一个值找不到匹配的索引值，而其他配对的值进行^操作都是0，0^剩下的那个数还等于那个数本身，这样就找到了缺失的值，时间复杂度是O(n)，空间复杂度是O(1)。

  ```
    value:[0,1,3,4]
    index:[0,1,2,3]

    =4∧(0∧0)∧(1∧1)∧(2∧3)∧(3∧4)
    =(4∧4)∧(0∧0)∧(1∧1)∧(3∧3)∧2
    =0∧0∧0∧0∧2
    =2
​	
	public int missingNumber(int[] nums) {

		int miss = nums.length;
		for (int i = 0; i < nums.length; i++) {
			miss ^= i ^ nums[i];
		}
		return miss;
	}

  ```