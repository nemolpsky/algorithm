## Find All Numbers Disappeared in an Array


Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

- Example:

  Input:

  ```
  [4,3,2,7,8,2,3,1]
  ```

  Output:
  
  ```
  [5,6]
  ```

---

## 解题思路

这道题目的意思是给定一个数组，里面数字的范围是[1,N]，但是会有重复的数字，需要找出这个范围内缺少的数字，要求空间复杂度是O(1)。

- set

  虽然要求空间复杂度是O(1)，但还是给一个最简单的空间复杂度为O(n)的解法，先保存所有的数字，再遍历数值的范围查看哪个数字是set中没有的，时间复杂度是O(n)。

  ```
    public List<Integer> findDisappearedNumbers(int[] nums) {
        // set保存数字
		HashSet<Integer> set = new HashSet<>();
		for (int i : nums) {
			set.add(i);
		}

		List<Integer> list = new ArrayList<>();
        // 遍历判断哪个数字没有
		for (int i = 1; i <= nums.length; i++) {
			if (!set.contains(i)) {
				list.add(i);
			}
		}

		return list;
    }
  ```

- 鸽子洞加异或运算符

  一个索引值只能对应的一个正确的值，所以只要先把正确的值放在正确的位置，其他不正确的就是缺失的值。

  ```
    public static List<Integer> findDisappearedNumbers(int[] nums) {
		// 遍历数组
		for (int i = 0; i < nums.length; i++) {
			// nums[i] - 1保证了索引不会越界
            // 只要值不符合正确的排序就进行异或交换
			while (nums[nums[i] - 1] != nums[i]) {
				swap(nums, i, nums[i] - 1);
			}
		}

        // 查看哪个数组和索引的规则是不正常的就是缺失的
		List<Integer> res = new ArrayList<>();
		for (int i = 0; i < nums.length; i++) {
			if (nums[i] != i + 1) {
				res.add(i + 1);
			}
		}
		return res;
	}


    // 因为如果 a ^ b = c ，那么 a ^ c = b 与 b ^ c = a
    // a = a ^ b，这个时候a的值是c，也就是a ^ b的值
    // b = a ^ b，这个时候实际是b = c ^ b，b就等于a
    // a = a ^ b，这个时候实际是a = c ^ a，a就等于b
    // 这样就完成了替换，而且没有使用额外的空间度
	public static void swap(int[] nums, int index1, int index2) {
		if (index1 == index2) {
			return;
		}
		nums[index1] = nums[index1] ^ nums[index2];
		nums[index2] = nums[index1] ^ nums[index2];
		nums[index1] = nums[index1] ^ nums[index2];
	}
  ```