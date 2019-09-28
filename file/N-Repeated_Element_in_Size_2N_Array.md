## N-Repeated Element in Size 2N Array
In a array A of size 2N, there are N+1 unique elements, and exactly one of these elements is repeated N times.

Return the element repeated N times.

- xample 1:

  Input: [1,2,3,3]

  Output: 3

- Example 2:

  Input: [2,1,2,5,3,2]

  Output: 2

- Example 3:

  Input: [5,1,5,2,5,3,5,4]

  Output: 5
 
- Note:
  - 4 <= A.length <= 10000
  - 0 <= A[i] < 10000
  - A.length is even

---

## 结题思路
这道题目就比较有意思，说给一个数组，数组长度是偶数，如果长度是2N，那么就会有N+1个唯一元素，还会有N个重复元素，要求找到这个重复元素

- 遍历+Set去重

  最简单的方法，遍历元素，放入Set集合中，每次都判断有没有值，如果有就表示重复了，也就找到了重复的元素，时间复杂度是O(n)。

  ```
  public static int repeatedNTimes(int[] A) {
	  HashSet<Integer> set = new HashSet<>();
	  for (int i = 0; i < A.length; i++) {
		  int val = A[i];
		  if (set.contains(val)) {
			  return val;
		  } else {
			  set.add(A[i]);
		  }
	  }
	  return 0;
  }
  ```

- 鸽子洞理论

  若有n个笼子和n+1只鸽子，所有的鸽子都被关在鸽笼里，那么至少有一个笼子有至少2只鸽子。我们会发现这道题目刚好可以符合这个定律，也就是说一个元素下面三个元素一定会有重复的。这是网上找的一个例子，可以参考下，这个方法还是比较绕的。理论上时间复杂度也是O(n)，空间复杂度则是O(1)。

  ```
    public static int repeatedNTimes2(int[] A) throws InterruptedException {
		for (int i = 0; i < A.length - 1; i++) {
			if (A[i] == A[i + 1]) { // any two neighbour numbers
				return A[i];
			}
		}
		// 2,1,2,5,3,2
		// could be evenly distributed excluding the above case
		for (int i = 0; i < A.length - 2; i++) {
			if (A[i] == A[A.length - 1] || A[i] == A[A.length - 2]) {
				return A[i];
			}
		}
		return 0;
	}
  ```