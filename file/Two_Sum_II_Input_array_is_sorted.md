## Two Sum II - Input array is sorted

Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

- Note:

  Your returned answers (both index1 and index2) are not zero-based.

  You may assume that each input would have exactly one solution and you may not use the same element twice.

- Example:

  Input: numbers = [2,7,11,15], target = 9

  Output: [1,2]

  Explanation: 
  
  The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.

---

## 解题思路

这道题给定一个包含负数，正数的数组，按升序排列，给定一个目标值，找到两个相加等于目标值的元素的索引。


- 数组存储位置

  两数求和的题目其实已经有过一道了，前面那道就是这样的解法，使用map或数组保存所有元素，这样每次对比的操作都是O(1)，只需要遍历两边，时间复杂度是O(n)。
  
  ```
	public int[] twoSum(int[] numbers, int target) {

		// 存储位置，因为有负数，所以从数组的一般作为0，前面的是负数，后面的是正数
		int[] position = new int[5000];
		for (int i = 0; i < numbers.length; i++) {
			position[numbers[i] + 2499] = i;
		}

		
		int[] arr = new int[2];
		// 遍历数组，如果目标值减去当前值的差值能够在数组中找到就表示这两个数可以累加成目标值
		for (int i = 0; i < numbers.length; i++) {
			int value = target - numbers[i];
			if (position[value + 2499] != 0) {
				arr[0] = i + 1;
				arr[1] = position[value + 2499] + 1;
				break;
			}
		}

		return arr;
	}

  ```

- 双指针

  题目中着重强调了数组是升序排列的，这点可以好好利用，可以从两头开始对比，如果两数之和比目标值大，就往前移动右指针，如果小则往后移动左指针，因为排序的特点，所以这样可以保证每次值都是最小范围的变动，不会错过目标值。

  ```
	public int[] twoSum(int[] numbers, int target) {

		// 左右索引，从两头开始
		int startIndex = 0;
		int endIndex = numbers.length - 1;

		int[] arr = new int[2];
		// 左右索引还没有碰到一起，表示还没遍历完
		while (startIndex < endIndex) {
			// 找到符合条件的两个值
			if (target == numbers[startIndex] + numbers[endIndex]) {
				arr[0] = startIndex + 1;
				arr[1] = endIndex + 1;
			}

			// 目标值比两个值大，就移动左边索引，相当于加一个比当前左边的值要大的最小的值尝试
			if (target > numbers[startIndex] + numbers[endIndex]) {
				startIndex++;
			}
			// 目标值比两个值小，就移动右边索引，相当于加一个比当前右边的值要小的最大的值尝试
			else {
				endIndex--;
			}
		}

		return arr;
	}
  ```