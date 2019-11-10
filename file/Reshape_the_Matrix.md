## Reshape the Matrix

In MATLAB, there is a very useful function called 'reshape', which can reshape a matrix into a new one with different size but keep its original data.

You're given a matrix represented by a two-dimensional array, and two positive integers r and c representing the row number and column number of the wanted reshaped matrix, respectively.

The reshaped matrix need to be filled with all the elements of the original matrix in the same row-traversing order as they were.

If the 'reshape' operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

- Example 1:

  Input: 
  ```
  nums = 
  [[1,2],
  [3,4]]
  r = 1, c = 4
  ```
  
  Output: 
  ```
  [[1,2,3,4]]
  ```

  Explanation:
  
  The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.

- Example 2:

  Input: 
  ```
  nums = 
  [[1,2],
  [3,4]]
  r = 2, c = 4
  ```

  Output: 
  ```
  [[1,2],
  [3,4]]
  ```

  Explanation:

  There is no way to reshape a 2 * 2 matrix to a 2 * 4 matrix. So output the original matrix.

- Note:
  - The height and width of the given matrix is in range [1, 100].
  - The given r and c are all positive.
---

## 解题思路
这道题的意思是给定一个二维数组，再给定两个值，这两个值代表一个新的二维数组的长宽，需要判断能不能将旧的二维数组转换为这个长宽的新二维数组，如果可以的话就返回新的数组，否则返回旧的数组。

- 双层遍历

  首先判断新数组和给定的两个值的积是否相同，相同就表示可以转换，然后遍历旧数组，再计算好索引值存入到新数组中，时间复杂度是O(n²)。

  ```
	public static int[][] matrixReshape(int[][] nums, int r, int c) {
		// 判断是否能组成新的矩阵
		int oldSize = nums.length * nums[0].length;
		int newSize = r * c;
		if (oldSize != newSize) {
			return nums;
		}

		// 新矩阵的长宽
		int[][] newArr = new int[r][c];
		// 列索引
		int rIndex = 0;
		// 行索引
		int cIndex = 0;
		// 遍历原数组
		for (int i = 0; i < nums.length; i++) {
			for (int l = 0; l < nums[0].length; l++) {
				// 如果行索引大于等于长度
				if (cIndex <= c) {
					// 小于长度，直接放入
					if (cIndex < c) {
						newArr[rIndex][cIndex] = nums[i][l];
						// 行索引往后推
						cIndex++;
					} 
					// 等于长度，修改到下一列再放入
					else {
						// 移动到下一列的0索引位置
						rIndex++;
						cIndex = 0;
						newArr[rIndex][cIndex] = nums[i][l];
						// 行索引往后推
						cIndex++;
					}
				}
			}
		}

		return newArr;
	}
  ```