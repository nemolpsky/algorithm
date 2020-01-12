## Maximize Sum Of Array After K Negations

Given an array A of integers, we must modify the array in the following way: we choose an i and replace A[i] with -A[i], and we repeat this process K times in total.  (We may choose the same index i multiple times.)

Return the largest possible sum of the array after modifying it in this way.

 

- xample 1:

  Input: A = [4,2,3], K = 1

  Output: 5

  Explanation: Choose indices (1,) and A becomes [4,-2,3].

- Example 2:

  Input: A = [3,-1,0,2], K = 3

  Output: 6

  Explanation: Choose indices (1, 2, 2) and A becomes [3,1,0,2].

- Example 3:

  Input: A = [2,-3,-1,5,-4], K = 2

  Output: 13

  Explanation: Choose indices (1, 4) and A becomes [2,3,-1,5,4].
 

- Note:

  - 1 <= A.length <= 10000
  - 1 <= K <= 10000
  - -100 <= A[i] <= 100

---

## 解题思路

这道题就比较有意思，是给定一个数组，数值的范围是[-100,100]，长度是10000，要求每次对数组中的一个元素进行正负转换(A[i] -> -A[i])，转换K次，要求转换尽量保证数组之和最大。

- 数组保存位置

  这道题的思路就是首先把负数都转换成正的，然后再对一个最小数进行反复转换即可，所以肯定是要排序的，但是如果用数组来保存元素的出现次数的话，就其实已经天然排序了，所以每次都对最小的数字进行操作翻转，但是要注意的是，假设[4,2,3]数组，排序之后是[2,3,4]，修改2成-2之后最小的数变成了-2，而不是3，所以只需要注意这一点即可，时间复杂度是O(n)。

  ```
	public int largestSumAfterKNegations(int[] A, int K) {

		// 存储数组出现个数，[-100,100]刚好需要200个位置
		int[] count = new int[201];

		// 因为有负数，所以-100存到0的位置，0存到101的位置，100存到201的位置
		for (int i : A) {
			count[i + 100]++;
		}

		int index = 0;
		// 转换K次
		while (K > 0) {
			// 0表示没这个元素，直到找到存在的元素
			while (count[index] == 0) {
				index++;
			}

			// 将这个元素次数减1，再将翻转正负号之后的数字次数加1
			count[index]--;
			count[200 - index]++;

			// 如果大于100表示是个正数，会多一个与之对应的负数，最小数是这个数
			if (index > 100) {
				index = 200 - index;
			}

			K--;
		}

		// 求和
		int sum = 0;
		for (int i = 0; i < count.length; i++) {
			sum += (i - 100) * count[i];
		}

		return sum;
	}

  ```