## Minimum Absolute Difference

Given an array of distinct integers arr, find all pairs of elements with the minimum absolute difference of any two elements. 

Return a list of pairs in ascending order(with respect to pairs), each pair [a, b] follows

a, b are from arr
a < b
b - a equals to the minimum absolute difference of any two elements in arr
 

- Example 1:

  Input: arr = [4,2,1,3]

  Output: [[1,2],[2,3],[3,4]]

  Explanation: The minimum absolute difference is 1. List all pairs with difference equal to 1 in ascending order.

- Example 2:

  Input: arr = [1,3,6,10,15]

  Output: [[1,3]]

- Example 3:

  Input: arr = [3,8,-10,23,19,-4,-14,27]

  Output: [[-14,-10],[19,23],[23,27]]
 

- Constraints:

  - 2 <= arr.length <= 10^5
  - -10^6 <= arr[i] <= 10^6

---

## 解题思路

这道题的意思是给定一个数组，数组里的所有值都不重复，然后任意取两个元素获取绝对差，知道找到最小的绝对差，再列出所有有相同绝对差的元素组合。

- 排序遍历

  一开始看到任意两个元素组合，就会觉得无从下手，但是其实注意求的是绝对差，也就是说如果先对数组进行排序，只要遍历对比每个元素和它下一个元素即可，遇到更小的就清空前面保存的元素对。

  ```
	public static List<List<Integer>> minimumAbsDifference(int[] arr) {
        // 排序
		Arrays.sort(arr);
        // 存放元素对的数组
		List<List<Integer>> list = new ArrayList<>();
        // 最小绝对差，初始化是Ineger的最大值
		int minimum = Integer.MAX_VALUE;

        // 遍历
		for (int i = 0; i < arr.length - 1; i++) {
            // 获取每个元素的和下一个元素的绝对差
			int abs = Math.abs(arr[i + 1] - arr[i]);

            // 小于等于
			if (abs<=minimum) {
                // 小于就清空集合重新保存
				if (abs<minimum) {
					minimum = abs;
					list = new ArrayList<>();
				}
                // 相等就存到现在的集合中
				list.add(Arrays.asList(new Integer[] { arr[i], arr[i + 1] }));
			}
		}

		return list;
	}
  ```

