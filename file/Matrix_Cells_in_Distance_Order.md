## Matrix Cells in Distance Order

We are given a matrix with R rows and C columns has cells with integer coordinates (r, c), where 0 <= r < R and 0 <= c < C.

Additionally, we are given a cell in that matrix with coordinates (r0, c0).

Return the coordinates of all cells in the matrix, sorted by their distance from (r0, c0) from smallest distance to largest distance.  Here, the distance between two cells (r1, c1) and (r2, c2) is the Manhattan distance, |r1 - r2| + |c1 - c2|.  (You may return the answer in any order that satisfies this condition.)

 

- Example 1:

  Input: R = 1, C = 2, r0 = 0, c0 = 0

  Output: [[0,0],[0,1]]

  Explanation: The distances from (r0, c0) to other cells are: [0,1]

- Example 2:

  Input: R = 2, C = 2, r0 = 0, c0 = 1

  Output: [[0,1],[0,0],[1,1],[1,0]]

  Explanation: The distances from (r0, c0) to other cells are: [0,1,1,2]

  The answer [[0,1],[1,1],[0,0],[1,0]] would also be accepted as correct.

- Example 3:

  Input: R = 2, C = 3, r0 = 1, c0 = 2

  Output: [[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]

  Explanation: The distances from (r0, c0) to other cells are: [0,1,1,2,2,3]

  There are other answers that would also be accepted as correct, such as [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]].
 

- Note:

  - 1 <= R <= 100
  - 1 <= C <= 100
  - 0 <= r0 < R
  - 0 <= c0 < C
---

## 解题思路

这道题就比较有意思，给定四个参数，R、C、r0和c0，首先要建立一个R*C的二维数组，然后[r0,c0]是数值中一个点，需要按其他的点到这个点的距离升序排列出一个二维数组，计算距离的公式已经给出，|r1 - r2| + |c1 - c2|。

- 遍历加排序

  既然最后输出的是一个排序的数组，那就需要先获取到数组，所以遍历数组来填充一个未排序的数组，最后按这个公式排序就可以，时间复杂度是O(n)，n=R*C。

  ```
	public static int[][] allCellsDistOrder(int R, int C, int r0, int c0) {

		int[][] arrRC = new int[R * C][2];

		int index = 0;
		for (int i = 0; i < R; i++) {
			for (int l = 0; l < C; l++) {
				arrRC[index][0] = i;
				arrRC[index][1] = l;
				index++;
			}
		}


		Arrays.sort(arrRC, new Comparator<int[]>() {

			@Override
			public int compare(int[] o1, int[] o2) {
				return Math.abs(o1[0] - r0) + Math.abs(o1[1] - c0) - (Math.abs(o2[0] - r0) + Math.abs(o2[1] - c0));
			}
		});

		return arrRC;
	}
  ```