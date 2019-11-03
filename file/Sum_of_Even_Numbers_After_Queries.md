## Sum of Even Numbers After Queries

We have an array A of integers, and an array queries of queries.

For the i-th query val = queries[i][0], index = queries[i][1], we add val to A[index].  Then, the answer to the i-th query is the sum of the even values of A.

(Here, the given index = queries[i][1] is a 0-based index, and each query permanently modifies the array A.)

Return the answer to all queries.  Your answer array should have answer[i] as the answer to the i-th query.

 

- Example 1:

  Input: A = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]

  Output: [8,6,2,4]

  Explanation: 

  At the beginning, the array is [1,2,3,4].
  After adding 1 to A[0], the array is [2,2,3,4], and the sum of even values is 2 + 2 + 4 = 8.
  After adding -3 to A[1], the array is [2,-1,3,4], and the sum of even values is 2 + 4 = 6.
  After adding -4 to A[0], the array is [-2,-1,3,4], and the sum of even values is -2 + 4 = 2.
  After adding 2 to A[3], the array is [-2,-1,3,6], and the sum of even values is -2 + 6 = 4.
 

- Note:

  - 1 <= A.length <= 10000
  - -10000 <= A[i] <= 10000
  - 1 <= queries.length <= 10000
  - -10000 <= queries[i][0] <= 10000
  - 0 <= queries[i][1] < A.length

---
## 解题思路

这套题其实并不难就是意思很啰嗦，给定一个一维数组和一个二维数组，遍历二维数组，获取到的也是一个一维数组，其中索引0上是要加的值，索引1上则是要把这个值加到前面那个一维数组的索引值，加完之后需要计算出改变之后的一维数组中所有的偶数和，放入一个新的数组中，索引和当前遍历到的二维数组的索引一样。

- 在初始的偶数和上进行增减

  这么长的一堆步骤其实并不难，只是麻烦一点，难点在于对数组的每次改变都是永久的，也就是说第二次的操作是基于第一次的，第一次操作可能会改变偶数的个数。所以在一开始就计算出所有的偶数和，后面的变化中如果被改变的元素本来就是偶数就减掉它，再看改变之后的新值是不是偶数，如果是的话就再加到偶数和之中，这样就可以动态的计算所有索引下的偶数和。

  ```
	public static int[] sumEvenAfterQueries(int[] A, int[][] queries) {
		
		int[] arr = new int[A.length];

		// 找出所有偶数之和
		int envens = 0;
		for (int i : A) {
			if (i % 2 == 0) {
				envens += i;
			}
		}

		// 遍历
		for (int i = 0; i < A.length; i++) {
			// 需要加值的索引
			int index = queries[i][1];
			// 需要加的值
			int num = queries[i][0];

			// 原来的值已经是偶数要减去
			int oldValue = A[index];
			if (oldValue % 2 == 0) {
				envens -= oldValue;
			}

			// 新值是偶数要加上
			int newValue = oldValue + num;
			if (newValue % 2 == 0) {
				envens += newValue;
			}

			// 修改旧数组
			A[index] = newValue;
			// 放置到新数组
			arr[i] = envens;
		}

		return arr;
	}
  ```