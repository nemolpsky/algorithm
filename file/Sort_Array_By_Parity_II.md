## Sort Array By Parity II

Given an array A of non-negative integers, half of the integers in A are odd, and half of the integers are even.

Sort the array so that whenever A[i] is odd, i is odd; and whenever A[i] is even, i is even.

You may return any answer array that satisfies this condition.

 

- Example 1:

  Input: [4,2,5,7]

  Output: [4,5,2,7]

  Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.
 

- Note:
  
  - 2 <= A.length <= 20000
  - A.length % 2 == 0
  - 0 <= A[i] <= 1000

---

## 解题思路
这道题目又是一个根据特定要求排序的题目，给定一个数组，不包含负数，数组长度一定是偶数，规则是如果当前索引是偶数那索引上的值也必须是偶数，奇数索引也是同理。

- 遍历存放到新数组中

  分别定义两个变量来记录奇数索引和偶数索引，遍历数组，存放到对应的索引中，再自增到下一个奇数索引或偶数索引。时间复杂度是O(n)，空间复杂度也是O(n)。

  ```
	public static int[] sortArrayByParityII(int[] A) {
		int[] arr = new int[A.length];

		int evenIndex = 0;
		int oddIndex = 1;

		for (int i = 0; i < A.length; i++) {
			int value = A[i];
			if (value % 2 == 0) {
				arr[evenIndex] = value;
				evenIndex += 2;
			} else {
				arr[oddIndex] = value;
				oddIndex += 2;
			}
		}

		return arr;
	}
  ```

- 遍历修改原数组

  这个方法会比上面的方法稍微复杂一点点，但是原理是一样的，同样是标记一个索引用来记录奇数查找位置。然后遍历偶数位置的元素，如果发现偶数位置上的值不合规则就去按照奇数索引去查询奇数的值，如果奇数索引的值也刚好不符合标准就互换，如果奇数索引上的值符合标准就查下一个奇数位置上的值。

  ```
	public static int[] sortArrayByParityII(int[] A) {
		int oddIndex = 1;

		for (int i = 0; i < A.length; i += 2) 
		{
			int evenValue = A[i];
			if (evenValue % 2 == 1) {
				while (A[oddIndex] % 2 == 1) {
					oddIndex += 2;
				}
				int tmp = A[i];
				A[i] = A[oddIndex];
				A[oddIndex] = tmp;
			}
		}

		return A;
	}
  ```