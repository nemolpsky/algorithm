## Height Checker

Students are asked to stand in non-decreasing order of heights for an annual photo.

Return the minimum number of students not standing in the right positions.  (This is the number of students that must move in order for all students to be standing in non-decreasing order of height.)


- Example 1:

  Input: [1,1,4,2,1,3]

  Output: 3

  Explanation: 

  Students with heights 4, 3 and the last 1 are not standing in the right positions.
 

- Note:
  - 1 <= heights.length <= 100
  - 1 <= heights[i] <= 100

---

## 解题思路

这道题目的本意非常简单，但是写的也很模糊。不知道是不是因为很多人没看懂题意的缘故，提交数量突然少了很多。其实就是给定一个无序的int数组，然后按升序排序，再对比排序前后有多少个元素变化，也就是对比两个数组相同索引不同元素的个数有多少个。

- 排序再对比

  这个是最简单的，先copy个新数组出来，再排序，再遍历对比一下，因为用的是自带的快排算法，所以时间复杂度是O(nlogn)。

  ```
	public static int heightChecker(int[] heights) {
		// copy一个新数组用于对比
		int[] newHeights = Arrays.copyOf(heights, heights.length);

		// 排序
		Arrays.sort(newHeights);

		// 对比排序后和未排序后的元素位置差异数量
		int count = 0;
		for (int i = 0; i < heights.length; i++) {
			if (heights[i] != newHeights[i]) {
				count++;
			}
		}
		return count;
	}
  ```

- 另类排序对比

  这是看到的另一个答案，很巧妙，先是建立一个数组来存放所有的元素的出现次数，相当于把这个数组当成一个天然的Map使用，索引就是key，value就是次数，而且还是升序的。然后循环这个map对每个key进行自减操作，减到0就跳到下一个操作。这其实就相当于一个升序遍历了，然后每次遍历再对比原数组的对应索引是不是该key，如果不是的话就表示这个元素在排序的时候需要被移动。因为只需要做两次遍历，所以时间复杂度是O(n)，速度更快。

  ```
	public static int heightChecker1(int[] heights) {
		// 建立一个数组，存放原来数组中各元素的出现次数，因为元素是从1开始，个数不超过100，所以该数组长度为N+1，也就是101
		int[] intMap = new int[101];
		for (int i : heights) {
			intMap[i]++;
		}

		// 计算不同数量和当前所自减的数字索引
		int res = 0, currHeight = 0;
		// 遍历数组
		for (int i = 0; i < heights.length; i++) {
			// 对比上面的存储数组中每个元素的数组，因为是按索引来当做key代表每个元素的出现次数，
			// 所以如果把这个数据按索引遍历，然后每次都对每个索引减1，减到0就跳到下一个索引，其实是按照数字的从小到大的排序进行的
			// 而原数组没有排序，所以每次只要两个进行对比，只要原数组的值和对新数组的值不一样就表示这个值是需要移动排序的，数量就加1
			// 这样是一种取巧的类似排序对比的方式
			
			while (intMap[currHeight] == 0) {
				currHeight++;
			}
			if (currHeight != heights[i]) {
				res++;
			}
			intMap[currHeight]--;
		}
		return res;
	}
  ```