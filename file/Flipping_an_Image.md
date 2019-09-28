## Flipping an Image
Given a binary matrix A, we want to flip the image horizontally, then invert it, and return the resulting image.

To flip an image horizontally means that each row of the image is reversed.  For example, flipping [1, 1, 0] horizontally results in [0, 1, 1].

To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting [0, 1, 1] results in [1, 0, 0].

- Example 1:

  Input: [[1,1,0],[1,0,1],[0,0,0]]

  Output: [[1,0,0],[0,1,0],[1,1,1]]

  Explanation: 
  
  First reverse each row: 
  [[0,1,1],[1,0,1],[0,0,0]].

  Then, invert the image: 
  [[1,0,0],[0,1,0],[1,1,1]]

- Example 2:

  Input: [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]

  Output: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]

  Explanation: 
  
  First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].

  Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]

- Notes:
  - 1 <= A.length = A[0].length <= 20
  - 0 <= A[i][j] <= 1

---

## 解题思路
这道题的意思是接受一个二维数组，然后做两步操作，第一步是每个内层数组的顺序先颠倒一遍，然后再将每个内层数组中的值进行转换，1转换为0，0转换为1

- 遍历修改值，倒序存放

  重点是内层数组的值和顺序，所以新建相同大小的数组，遍历的时候把内层数组的值先转换再倒着存放到新的数组即可，遍历次数是所有子数组的元素之和，所以时间复杂度是O(n)

  ```
	public static int[][] flipAndInvertImage(int[][] A) {
		// 创建相同大小的返回的外层数组
		int[][] outArr = new int[A[0].length][A.length];
		// 遍历外层数组
		for (int l = 0; l < A.length; l++) {
			int[] arr = A[l];
			// 创建相同大小的返回的内层数组
			int[] newArr = new int[arr.length];
			// 遍历内层数组
			for (int i = 0; i < arr.length; i++) {
				//翻转值
				int value = arr[i] == 1 ? 0 : 1;
				//倒叙存入新的内层数组中
				newArr[arr.length - i - 1] = value;
			}
			//将新的内层数组存入新的外层数组
			outArr[l] = newArr;
		}

		return outArr;
	}
  ```

- 优化简写

  看到一种很好的解法，每次值遍历到中间的索引，如果元素个数是单数，中间那个不需要调换位置，在遍历前半部分的时候就进行值的转换，然后再和后半部分对应的元素互换值，```int tmp = row[i] ^ 1```转换值的写法也比上面三元表达式的效率要好

  ```
    public static int[][] flipAndInvertImage1(int[][] A) {
    	// 子数组的长度
        int C = A[0].length;
        // 遍历修改每个子数组
        for (int[] row: A)
        	// 长度为3
        	// i = 0; i <3+1/2; 1->0
        	// i = 1; i <3+1/2
        	// i = 2; i <3+1/2
            for (int i = 0; i < (C + 1) / 2; ++i) {
            	// 转换后的值
                int tmp = row[i] ^ 1;
                // 根据中点调换位置，然后放置转换后的值
                row[i] = row[C - 1 - i] ^ 1;
                row[C - 1 - i] = tmp;
            }

        return A;
    }
  ```