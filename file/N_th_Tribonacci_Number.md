## N-th Tribonacci Number

The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given n, return the value of Tn.

 

- Example 1:

  Input: n = 4

  Output: 4

  Explanation:

  T_3 = 0 + 1 + 1 = 2

  T_4 = 1 + 1 + 2 = 4

- Example 2:

  Input: n = 25

  Output: 1389537
 
- Constraints:

  0 <= n <= 37,The answer is guaranteed to fit within a 32-bit integer, ie. answer <= 2^31 - 1.

---

## 解题思路

这道题真的非常有意思，题目本身是挺简单的，其实就是给定一个特殊的数列，然后给定数列的长度，求出整个数列的和，但是整个数值的计算非常大，所以需要考虑缓存优化的思想。

- 递归加数组缓存

  题目本身很简单，就是一个递归，深度优先搜索，每个值都分解到最小的固定值，然后累加，但是需要注意的是这个值的递增是非常快的，n=4的时候f(n)=4，而n=25的时候f(n)=1389537，再往后直接超出了LeetCode的限定时间，所以需要加缓存，因为每个n都是前3个值相加，比如f(8)=f(7)+f(6)+f(5)，f(7)=f(6)+f(5)+f(4)，所以可以看到其实每个值都会被复用，只需要计算一次存起来就可以，时间复杂度是O(n)。

  ```

    static int arr[] = new int[38];  

	// T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.
	public static int tribonacci(int n) {
		int count = 0;
		return count(n, count);
	}

	public static int count(int n, int count) {
		// 小于0直接返回当前值
		if (n < 0) {
			return count;
		}

		// n等于0的时候
		if (n == 0) {
			return count;
		}

		// n等于1或2的时候加1
		if (n == 1 || n == 2) {
			count += 1;
			return count;
		}

		// 数组中没有值就需要计算再存入数组
		if (arr[n] == 0) {
			// n值 = (n-3) + (n-2) + (n-1)
			count = count(n - 1, count) + count(n - 2, count) + count(n - 3, count);
			arr[n] = count;
		}
		// 数组中已经有值了就不需要再计算了
		else {
			count = arr[n];
		}

		return count;
	}

  ```