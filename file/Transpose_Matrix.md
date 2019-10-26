## Transpose Matrix

Given a matrix A, return the transpose of A.

The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.


![hint_transpose](https://github.com/nemolpsky/algorithm/raw/master/file/image/hint_transpose.png)


- Example 1:

  Input: [[1,2,3],[4,5,6],[7,8,9]]

  Output: [[1,4,7],[2,5,8],[3,6,9]]

- Example 2:

  Input: [[1,2,3],[4,5,6]]

  Output: [[1,4],[2,5],[3,6]]
 

- Note:

  - 1 <= A.length <= 1000
  - 1 <= A[0].length <= 1000

---

## 解题思路

这道题的意思就是把二维数组的行和列颠倒过来。

- 遍历

  颠倒后二维数组的x、y坐标刚好反过来了，所以遍历的时候把坐标反过来存入新数组中，新数组的行列长度也互换，时间复杂度是O(n²)

  ```
	public static int[][] transpose(int[][] A) {

		int[][] arr = new int[A[0].length][A.length];

		for (int i = 0; i < A.length; i++) {
			for (int l = 0; l < A[0].length; l++) {
				arr[l][i] = A[i][l];
			}
		}

		return arr;
	}
  ```