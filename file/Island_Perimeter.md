## Island Perimeter

You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

 
![岛屿](https://github.com/nemolpsky/algorithm/raw/master/file/image/island.png)

- Example:

  Input:
  ```
  [[0,1,0,0],
   [1,1,1,0],
   [0,1,0,0],
   [1,1,0,0]]
  ```
  Output: 16

  Explanation: The perimeter is the 16 yellow stripes in the image below:


---

## 解题思路

这道题非常有意思，给定一个二维数组，其中1代表图中的黄块，0则代表蓝块，黄块则组成岛屿，要求根据二维数组算出图中岛屿的周长。

- 遍历计算每个黄块的周长

  其实可以发现一个规律，如果一个黄块附近有一个黄块它的周长就减1，如果全都包住则4条边的周长都不计入，所以只需要找到每个黄块，然后计算出它的周长再相加就可以，时间复杂度是O(n*m)。

  ```
	public static int islandPerimeter(int[][] grid) {

		int row = grid.length;
		int column = grid[0].length;

		int sum = 0;

		for (int i = 0; i < row; i++) {
			for (int l = 0; l < column; l++) {
				// 判断上下左右的值，如果遇到同样是1就减1
				int count = 0;
				if (grid[i][l] == 1) {
					count = 4;
					if (i > 0 && grid[i - 1][l] == 1) {// 上
						count--;
					}
					if (i < row - 1 && grid[i + 1][l] == 1) {// 下
						count--;
					}
					if (l > 0 && grid[i][l - 1] == 1) {// 左
						count--;
					}
					if (l < column - 1 && grid[i][l + 1] == 1) {// 右
						count--;
					}
				}
				sum += count;
			}
		}

		return sum;
	}
  ```

- 遍历计算两边周长再乘以2

  还可以发现一个规律，就是岛屿的上下两面之间的周长和左右两面之间的周长都是一样的，所以只需要算出两边的周长再乘以2即可，或者说每次减边的时候都减去2，时间复杂度是O(n*m)。

  ```
	public static int islandPerimeter(int[][] grid) {

		int row = grid.length;
		int column = grid[0].length;

		int sum = 0;

		for (int i = 0; i < row; i++) {
			for (int l = 0; l < column; l++) {
				// 判断上下左右的值，如果遇到同样是1就减1
				int count = 0;
				if (grid[i][l] == 1) {
					count = 4;
					if (i > 0 && grid[i - 1][l] == 1) {// 上
						count-=2;
					}

					if (l > 0 && grid[i][l - 1] == 1) {// 左
						count-=2;
					}
				}
				sum += count;
			}
		}

		return sum;
	}
  ```