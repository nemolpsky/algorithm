## Surface Area of 3D Shapes

On a N * N grid, we place some 1 * 1 * 1 cubes.

Each value v = grid[i][j] represents a tower of v cubes placed on top of grid cell (i, j).

Return the total surface area of the resulting shapes.


- Example 1:

  Input: [[2]]

  ![example1](https://github.com/nemolpsky/algorithm/raw/master/file/image/3d_area1.png)

  Output: 10

- Example 2:

  Input: [[1,2],[3,4]]

  ![example2](https://github.com/nemolpsky/algorithm/raw/master/file/image/3d_area2.png)

  Output: 34

- Example 3:

  Input: [[1,0],[0,2]]

  Output: 16

- Example 4:

  Input: [[1,1,1],[1,0,1],[1,1,1]]

  Output: 32

- Example 5:

  Input: [[2,2,2],[2,1,2],[2,2,2]]
  
  Output: 46
 

- Note:

  - 1 <= N <= 50
  - 0 <= grid[i][j] <= 50
---

## 解题思路
这道题的意思是说给定一个二维数组，每个索引下代表的就是当前的方块数量，比如[[2]]就表示在索引[0,0]位置上有两个叠在一起的立方体，要求算出立方体的整个面积。

- 双层遍历

  这道题跟前面一道算岛屿周长的很像，只不过是立体的，更加复杂，但是原理也是一样，遍历所有的位置，然后拿总面积减去上下左右被遮挡住的面积，最后相加即可，时间复杂度是O(n)，n是i*j。

  ```
	public static int surfaceArea(int[][] grid) {

		int row = grid.length;
		int colunum = grid[0].length;

		int allCount = 0;

		// 遍历每个位置
		for (int i = 0; i < row; i++) {
			for (int l = 0; l < colunum; l++) {
				int value = grid[i][l];
				int count = 0;
				if (value != 0) {
					count = 6 * value - (value - 1) * 2;
					// 上
					if (i > 0) {
						int upValue = grid[i - 1][l];
						count -= Math.min(value, upValue);
					}
					// 下
					if (i < row - 1) {
						int downValue = grid[i + 1][l];
						count -= Math.min(value, downValue);
					}
					// 左
					if (l > 0) {
						int leftValue = grid[i][l - 1];
						count -= Math.min(value, leftValue);
					}
					// 右
					if (l < colunum - 1) {
						int rightValue = grid[i][l + 1];
						count -= Math.min(value, rightValue);
					}
				}
				System.out.println("[" + i + "," + l + "] : " + count);
				allCount += count;
			}
		}
		return allCount;
	}
  ```