## Intersection of Two Arrays II

Given two arrays, write a function to compute their intersection.

- Example 1:

  Input: nums1 = [1,2,2,1], nums2 = [2,2]

  Output: [2,2]

- Example 2:

  Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]

  Output: [4,9]

- Note:

  Each element in the result should appear as many times as it shows in both arrays.The result can be in any order.

- Follow up:

  - What if the given array is already sorted? How would you optimize your algorithm?
  - What if nums1's size is small compared to nums2's size? Which algorithm is better?
  - What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

---

## 解题思路

这道题是说求两个数组的交集，前面有过一道类似的题了，只不过这道题中的数组数值范围更大，还包含负数，而且并集不需要去重。


- HashMap

  因为数量特别大而且还包含负数，所以用map来存储一个集合的位置比较方便，然后使用另一个集合对比这个map，对比一次数值就减1，时间复杂度是O(n)。

  ```
	public int[] intersect(int[] nums1, int[] nums2) {
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
		HashMap<Integer, Integer> map = new HashMap<>();

		for (int i = 0; i < otherArr.length; i++) {
            // 出现一次次数加1
			int value = map.getOrDefault(otherArr[i], 0);
			map.put(otherArr[i], value + 1);
		}

		// 存放都有的数据
		int[] arr = new int[length];
		int index = 0;
		for (int i = 0; i < length; i++) {
			int value = map.getOrDefault(currentArr[i], 0);
			if (value >= 1) {
				arr[index] = currentArr[i];
                // 对比到一次就次数减1
				map.put(currentArr[i], value - 1);
				index++;
			}
		}

		return Arrays.copyOfRange(arr, 0, index);
	}
  ```