## Majority Element

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

- Example:

  Input: 38

  Output: 2 

  Explanation: 
  
  The process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.

- Follow up:

  - Could you do it without any loop/recursion in O(1) runtime?

---

## 解题思路

这道题目是给定一个数组，里面包含了各种数字，但是会有一个数字的次数超过了数组长度的一半，需要找出这个数字，要求空间复杂度是O(1)。

- map

  最简单的肯定就是map存储次数，然后找到最多次数的，时间复杂度是O(n)，但是空间复杂度也是O(n)。
  
  ```
	public static int majorityElement(int[] nums) {
		HashMap<Integer, Integer> map = new HashMap<>();
		// 存储每个数字出现的次数
		for (int i : nums) {
			int count = map.get(i) == null ? 0 : map.get(i);
			map.put(i, ++count);
		}

		// 出现次数最多的数字
		int result = -1;
		// 最大次数
		int max = -1;
		// 当前最大次数
		int currentMax = -1;
		// 遍历map
		for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
			// 找出最大次数
			currentMax = Math.max(currentMax, entry.getValue());
			
			// 每次最大次数改变的时候都修改key值
			if (currentMax != max) {
				result = entry.getKey();
				max = currentMax;
			}
		}

		return result;
	}
  ```

- 鸽子洞理论

  因为数组长度是n，而有数字是n+1，也就是说如果做抵消操作，比如拿到最多的数字+1，没拿到就-1，最后肯定肯定会剩1，所以可以按这个思路，从第一个数开始对比，遇到相同的就+1，不相同-1，小于0就换成对比当前数，到最后没有小于0的对比数一定是最多的那个数，因为这个数超过了一半，+1的次数比-1的次数多1，一定会剩下1，无论顺序如何，时间复杂度是O(n)，空间复杂度也是O(1)。

  ```

	public static int majorityElement2(int[] nums) {

		int index = 0;
		// 统计次数
		int count = 0;
		// 当前值
		int currentValue = nums[0];
		for (int i : nums) {
			// 相等次数加1
			if (i == currentValue) {
				count++;
			}
			// 不相等次数减1
			else {
				count--;
			}

			// 小于0重新选个值对比
			if (count < 0) {
				currentValue = nums[index];
				count = 0;
			}
			index++;
		}

		return currentValue;
	}
  ```