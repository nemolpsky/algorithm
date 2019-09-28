## Sort Array By Parity
Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.

You may return any answer array that satisfies this condition.

- Example 1:

  Input: [3,1,2,4]

  Output: [2,4,3,1]

  The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
 
- Note:
  - 1 <= A.length <= 5000
  - 0 <= A[i] <= 5000

---

## 解题思路
这道题的意思很简单，给定一个int数组，全都是正值，要求把所有的偶数放在前面，奇数放在后面，顺序没有要求

- 区分出奇数偶数重新排序
  
  思路还是比较清晰的，首先肯定是要遍历每个元素，判断是奇数还是偶数，偶数放前面，奇数放后面。所以可以从两边分别放元素，偶数从头开始放，奇数则从尾开始放，时间复杂度是O(n)。

  ```
  public int[] sortArrayByParity(int[] A) {
	int[] newArr = new int[A.length];

    // 记录放置奇数和偶数的索引位置
	int startIndex = 0;
	int endIndex = A.length - 1;
	
	for (int i = 0; i < A.length; i++) {
		int val = A[i];
		if (val % 2 == 0) {// 偶数
			newArr[startIndex] = val;
            //放置完后索引加1，往后走
			startIndex++;
		} else {// 奇数
			newArr[endIndex] = val;
            //放置完后索引减1，往前走
			endIndex--;
		}
	}

	return newArr;
  } 
  ```

