## Fibonacci Number

The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1.
Given N, calculate F(N).

- Example 1:

  Input: 2

  Output: 1

  Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.

- Example 2:

  Input: 3

  Output: 2

  Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.

- Example 3:

  Input: 4

  Output: 3

  Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
 

- Note:
  - 0 ≤ N ≤ 30.

---

## 解题思路
这道题是说的斐波那契数列，也就是每个元素都是前两个元素相加的和，也就是F(N)=F(N-1)+F(N-2)，要求给定一个N值，计算出F(N)的值。


- 递归

  既然已经知道F(N)=F(N-1)+F(N-2)的规律，递归就可以解决，主要注意的是当N是0和1的时候值比较特殊，所以特殊判断下这两个值即可，其他的直接走递归。这种方法因为是每个元素都要走两次递归，所以时间复杂度是O(n²)。

  ```
	// F(0) = 0; F(1) = 1;
	// F(2) = F(1) + F(0); 0 + 1 = 1;
	// F(3) = F(2) + F(1); 1 + 1 + 2;
	// F(4) = F(3) + F(2); 2 + 1 = 3;
	// F(N) = F(N - 1) + F(N - 2), for N > 1.
	public static int fib(int N) {
		if (N <= 1) {
			return N;
		}
		return fib(N - 1) + fib(N - 2);
	}
  ```

- 循环

  这种解决方法也是按照上面的规律解决的，因为只有F(0)和F(1)是需要特殊判断的，后面的都是叠加规则的，所以完全可以从F(3)开始for循环来计算，时间复杂度是O(n)。

  ```
    // F(0) = 0; F(1) = 1;
	// F(2) = F(1) + F(0); 0 + 1 = 1;
	// F(3) = F(2) + F(1); 1 + 1 + 2;
	// F(4) = F(3) + F(2); 2 + 1 = 3;
	// F(N) = F(N - 1) + F(N - 2), for N > 1.
	public static int fib(int N) {
		if (N <= 1) {
			return N;
		}

		if (N == 2) {
			return 1;
		}

		// 当前累加值
		int currentValue = 0;
		// N+1的值
		int NSubtract1 = 1;
		// N+2的值
		int Nsubtract2 = 1;

		// 从F(3)开始循环
		for (int i = 3; i <= N; i++) {
			// 当前的值累加值就是N-1 + N-2，例如F(3)=1+1
			currentValue = NSubtract1 + Nsubtract2;
			// 然后下一个的N-1就是N当前的N-2
			Nsubtract2 = NSubtract1;
			// 下一个的N-2就是当前的N-1
			NSubtract1 = currentValue;

		}

		return currentValue;
	}
  ```
