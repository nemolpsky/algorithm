## Largest Perimeter Triangle

Given an array A of positive lengths, return the largest perimeter of a triangle with non-zero area, formed from 3 of these lengths.

If it is impossible to form any triangle of non-zero area, return 0.

 

- Example 1:

  Input: [2,1,2]

  Output: 5

- Example 2:

  Input: [1,2,1]

  Output: 0

- Example 3:

  Input: [3,2,3,4]

  Output: 10

- Example 4:

  Input: [3,6,2,3]

  Output: 8

- Note:

  - 3 <= A.length <= 10000
  - 1 <= A[i] <= 10^6
 
---

## 解题思路
这道题是说给定一个数组，长度在[3,10000]之间，需要找到能组成三角形并且周长最长的三个值。

- 排序遍历

  首先要了解组成三角形的要求，即任意两边之和大于第三边，然后需要找到最长的组合，那就肯定是从大的值开始试，如果不满足再去尝试更小的值，麻烦在于10000个数之内找到满足条件的任意三个值的组合会非常麻烦，所以需要排序，从尾部三个值开始遍历，这样满足了条件一，从最大值开始试，而且因为排了序，所以只需要判断n < (n-1) + (n-2)，如果满足条件，其他两个组合就不需要试了，因为n > n+1 > n+2，两条小边加起来都大于最大边，那么其他小边加最大边也一定比另一条小边大。另外既然判断的是n < (n-1) + (n-2)，n-2后面的值都是更小的，两个大值都没法比n大，其他值也就没有必要尝试了，所以只需要遍历数组，每次判断数组的前两个即可，时间复杂度是O(n)。


  ```
	public static int largestPerimeter(int[] A) {
		// 排序
		Arrays.sort(A);

		int length = A.length;

        // 每次都看当前元素和前两个是否能组成三角形
		for (int i = length - 1; i >= 2; i--) {
			if (A[i] < A[i - 1] + A[i - 2]) {
				return A[i] + A[i - 1] + A[i - 2];
			}
		}
		return 0;
	}
  ```

