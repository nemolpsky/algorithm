## Single Number

Given a non-empty array of integers, every element appears twice except for one. Find that single one.

- Note:

  Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

- Example 1:

  Input: [2,2,1]
  
  Output: 1

- Example 2:

  Input: [4,1,2,1,2]

  Output: 4

---

## 解题思路
这道题是说给定一个数组，数组里面会有一个数字是只出现一次，找出这个数字，要求最好是时间复杂度是O(n)，空间复杂度是O(1)。

- 数学公式

  假设有一个数组是[a,b,c,a,c]，其实可以推导出2(a+b+c)-(a+a+b+b+c)=c，因为是把所有的值乘以2减去目前数组中的值，目前数值中是有一个数字只出现一次，所以差值就是这个数字。

  ```
	public int singleNumber(int[] nums) {
		HashSet<Integer> set = new HashSet<>();

		int sum1 = 0;
		for (int i : nums) {
			set.add(i);
			sum1 += i;
		}

		int sum2 = 0;
		for (int i : set) {
			sum2 += i;
		}

		return sum2 * 2 - sum1;
	}
  ```

- ^符号运算

  这个方法就真的非常巧妙了，首先^的作用是，比较两个数字的二进制值上所有位数值，如果相同返回0，不相同则返回1。比如8^11，就是1000和1011对比，所以返回了0011，也就是十进制的3。

  因为0的二进制是0000，所以任何数^0都是它自己，比如0000和1011对比还是1011，而任何数^自己都是0，因为完全相同，返回0000。换成公式表达就是a^0=a，a^a=0，所以如果a^b^c^a^b=c，因为这个表达式可以调换顺序成这样a^a^b^b^c，会发现前面都是0了，那c^0自然就是c了，时间复杂度是O(n)，空间复杂度是O(1)。

  ```
	public int singleNumber(int[] nums) {
		int result = 0;

		for (int i : nums) {
			result ^= i;
		}

		return result;
	}
  ```