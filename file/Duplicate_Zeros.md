## Duplicate Zeros


Given a fixed length array arr of integers, duplicate each occurrence of zero, shifting the remaining elements to the right.

Note that elements beyond the length of the original array are not written.

Do the above modifications to the input array in place, do not return anything from your function.

- Example 1:

  Input: [1,0,2,3,0,4,5,0]

  Output: null

  Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]

- Example 2:

  Input: [1,2,3]

  Output: null

  Explanation: After calling your function, the input array is modified to: [1,2,3]
 

- Note:

  - 1 <= arr.length <= 10000
  - 0 <= arr[i] <= 9
---

## 解题思路

这道题确实比较难了，意思是给定一个数组，里面包含了很多0，要求没遇到一个0就在0后面复写一个0，而原来位置上的数字则往后移动，这样数组必然会超出原来的长度，所以超出长度的那部分就不要了，而且还特别做了限制要求不能copy，只能在原数组上修改。

- 数学公式计算

  这道题在LeetCode上有解决答案，写的非常详细，还有图解，这里只做大概的讲解，首先因为要复制0，所以整个数组长度会增加，所以可以遍历算出到需要复制几个0就可以填满数组，然后停止遍历，停止的那个索引上元素就是要放到数组最后的位置，依次往前遍历，遇到0就放两个，遇到非0就直接放，所以复制0的数量和索引的计算必须完全正确否则就会出错，时间复杂度是O(n)，空间复杂度是O(1)。

  ```
	public static void duplicateZeros(int[] arr) {
		// 数组长度
		int length = arr.length - 1;
		// 需要复写0个数
		int zeroNum = 0;
		// 遍历数组，遍历的元素数量加上复写0的个数等于长度就表示可以填满数组了，不需要再继续遍历了
		for (int i = 0; i <= length - zeroNum; i++) {
			if (arr[i] == 0) {

				// 如果最后一个是0，需要特殊处理
				if (i == length - zeroNum) {
					// 如果最后一个是0直接放到尾部，数组长度也减1
					arr[length] = 0;
					length -= 1;
					break;
				}
				// 复写的0数量加1
				zeroNum++;
			}
		}

		int index = length - zeroNum;

		// 要移到尾部的索引
		for (int i = index; i >= 0; i--) {
			if (arr[i] == 0) {
				arr[zeroNum + i] = 0;
				arr[zeroNum + i - 1] = 0;
				zeroNum--;
			} else {
				arr[zeroNum + i] = arr[i];
			}
		}
	}
  ```
