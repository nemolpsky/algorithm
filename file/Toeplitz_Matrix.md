## Toeplitz Matrix

A matrix is Toeplitz if every diagonal from top-left to bottom-right has the same element.

Now given an M x N matrix, return True if and only if the matrix is Toeplitz.
 

- Example 1:

  Input:

  ```
  matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
  ]
  ```

  Output: True

  Explanation:

  In the above grid, the diagonals are:"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".In each diagonal all elements are the same, so the answer is True.

- Example 2:

  Input:

  ```
  matrix = [
  [1,2],
  [2,2]
  ]
  ```

  Output: False

  Explanation:

  The diagonal "[1, 2]" has different elements.

- Note:

  - matrix will be a 2D array of integers.
  - matrix will have a number of rows and columns in range [1, 20].
  - matrix[i][j] will be integers in range [0, 99].

- Follow up:

  - What if the matrix is stored on disk, and the memory is limited such that you can only load at most one row of the matrix into the memory at once?
  - What if the matrix is so large that you can only load up a partial row into the memory at once?

---

## 解题思路
这道题的意思是给定一个矩阵数组，如果每条对角线的值都一样，就返回true，否则返回false。其实就是检查一个点的左上邻点是否一样，一直检查到边界处停止。

- 遍历检查左上邻点

  遍历所有的节点，除了边界上那些没有左上邻点的点之外，其他的都直接检查自己的左上邻点，如果有不同就表示要返回false，时间复杂度是O(n*m)。

  ```
	public static boolean isToeplitzMatrix(int[][] matrix) {
		// 行
		int row = matrix.length - 1;
		// 列
		int column = matrix[0].length - 1;

		// 遍历
		for (int i = 0; i < row; i++) {
			for (int l = 0; l < column; l++) {
				// 如果i和l中有一个等于0就表示到边界了，这个点是没有左上邻点的
				if (i > 0 && l > 0) {
					System.out.println(i + "," + l);
					// 判断是否和左上邻点一样
					if (matrix[i][l] != matrix[i - 1][l - 1]) {
						return false;
					}
				}
			}
		}

		return true;
	}

  ```