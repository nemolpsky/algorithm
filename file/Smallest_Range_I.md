## Smallest Range I

Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.

 

- Example 1:

  Input: A = [1], K = 0

  Output: 0

  Explanation: B = [1]

- Example 2:

  Input: A = [0,10], K = 2

  Output: 6

  Explanation: B = [2,8]

- Example 3:

  Input: A = [1,3,6], K = 3

  Output: 0

  Explanation: B = [3,3,3] or B = [4,4,4]
 

- Note:

  - 1 <= A.length <= 10000
  - 0 <= A[i] <= 10000
  - 0 <= K <= 10000

---

这道题就是有点数学的理念在里面，给定一个数组和一个数字K，数组里的数字可以改为是在-K <= x <= K的范围值，再将这个值加x或减x，如果是在这个范围内就可以将这个元素加x或减x然后放入新的数组中，可以重复放入，比如上面A = [1,3,6], K = 3中，1和3都在[-3,3]的范围内，所以组成的新数组可以是[1+3,1+3,1+3]或[6-3,6-3,6-3]，然后再求这个数组中的最大值和最小值的差值，元素都一样自然就为0。而A = [0,10], K = 2则可以是[0+2,10-2]，所以差值是8-2=6。

- 公式计算

  上面两个例子就是需要处理的两种不同的情况，第一种就是数组中所有的值都可以更改，那最后新数组中的值就会是一样，这个时候差值就是0，另一种就是新数组中的值不同，就需要拿到最大值和最小值来对比了。判断的依据就是如果数组中的最大值和最小值还没有2K大，那就是表示所有元素都在可以更改的范围呢，否则就不是，时间复杂度是O(n)。

  ```
	public static int smallestRangeI(int[] A, int K) {

		int min = Integer.MAX_VALUE;
		int max = Integer.MIN_VALUE;

		// 找出最大值和最小值
		for (int i : A) {
			if (min > i) {
				min = i;
			}

			if (max < i) {
				max = i;
			}
		}

		// 数组内的元素都可以相同
		if (max - min <= 2 * K) {
			return 0;
		} else {
			// 不相同就拿最大值减最小值
			// 新数组的最大值就是原数组的最大值加K，最小值就是原数组的最小值-K
			return (max - K) - (min + K);
		}

	}
  ```