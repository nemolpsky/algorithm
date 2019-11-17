## Move Zeroes

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

- Example:

  Input: [0,1,0,3,12]

  Output: [1,3,12,0,0]

- Note:

  - You must do this in-place without making a copy of the array.
  - Minimize the total number of operations.

---

## 解题思路

这道题是给定一个数组，把所有的0都放后面，要求不copy数组。

- 遍历

  先移动非0，再填充0，因为题目要求尽量减少交换次数，所以还加了额外的判断，如果索引是相同的就不交换，时间复杂度是O(n)，n等于nums.length + 0的数量，空间复杂度是O(1)。

  ```
  	public static void moveZeroes(int[] nums) {

		// 统计0的数量
		int zeroNumber = 0;
		// 当前索引
		int index = 0;
		// 填写的索引
		int nonZeroIndex = 0;
		for (int i : nums) {
			// 如果同一个索引就不需要交换了
			if (index != nonZeroIndex) {
				// 不为0就依次排下来
				if (i != 0) {
					nums[nonZeroIndex] = i;
					nonZeroIndex++;
				} 
				// 为0则计算统计加1
				else {
					zeroNumber++;
				}
			} 
			// 同一个位置虽然不交换但是需要计算填充索引和0的数量
			else {
				if (i != 0) {
					nonZeroIndex++;
				} else {
					zeroNumber++;
				}
			}
			index++;
		}

		// 把0填满后面
		index = nums.length - zeroNumber;
		for (int i = index; i < nums.length; i++) {
			nums[i] = 0;
		}

	}
  ```