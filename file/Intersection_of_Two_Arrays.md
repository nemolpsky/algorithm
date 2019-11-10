## Intersection of Two Arrays

Given two arrays, write a function to compute their intersection.

- Example 1:

  Input: nums1 = [1,2,2,1], nums2 = [2,2]

  Output: [2]

- Example 2:

  Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]

  Output: [9,4]

- Note:

  - Each element in the result must be unique.
  - The result can be in any order.

---

## 解题思路
这道题就是给定两个数组，求交集。

- 数组保存对比
  
  最直接方法肯定是双层遍历，但那是最慢的，时间复杂度是O(n²)。首先需要知道求交集的话，取决于最小的那个数组，只需要遍历短数组在长数组中有多少也存在就可以，可以使用map或者数组遍历长数组，这样遍历短数组每个元素是否在长数组中也存在的操作时间复杂度就是O(1)，需要注意的是短数组中重复的元素要排除掉，最终的时间复杂度是O(n)。

  ```
	public static int[] intersection(int[] nums1, int[] nums2) {

		// 找出长度最小的数组
		int length = Math.min(nums1.length, nums2.length);
		// 当前遍历的数组，长度小的那个
		int[] currentArr = null;
		// 对比的数组，长度长的那个
		int[] otherArr = null;
		if (length == nums1.length) {
			currentArr = nums1;
			otherArr = nums2;
		} else {
			currentArr = nums2;
			otherArr = nums1;
		}

		// 统计对比的数组的出现的元素
		int[] otherCountArr = new int[1000];

		for (int i = 0; i < otherArr.length; i++) {
			otherCountArr[otherArr[i]] = 1;
		}

		// 存放都有的数据
		int[] arr = new int[length];
		int index = 0;
		// 遍历短数组
		for (int i = 0; i < length; i++) {
			// 对比在长数组是否有值
			if (otherCountArr[currentArr[i]] == 1) {
				// 有值就保存并且把对比数组中的值修改
				arr[index] = currentArr[i];
				otherCountArr[currentArr[i]] = 0;
				index++;
			}
		}

		// 短数组中可能有重复的，因此可能最终长度会比数组短，后面为0的全部截取掉
		return Arrays.copyOfRange(arr, 0, index);
	}
  ```