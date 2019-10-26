## Divisor Game

Alice and Bob take turns playing a game, with Alice starting first.

Initially, there is a number N on the chalkboard.  On each player's turn, that player makes a move consisting of:

Choosing any x with 0 < x < N and N % x == 0.
Replacing the number N on the chalkboard with N - x.
Also, if a player cannot make a move, they lose the game.

Return True if and only if Alice wins the game, assuming both players play optimally.

 

- Example 1:

  Input: 2

  Output: true

  Explanation: Alice chooses 1, and Bob has no more moves.

- Example 2:

  Input: 3

  Output: false

  Explanation: Alice chooses 1, Bob chooses 1, and Alice has no more moves.
 

- Note:
  - 1 <= N <= 1000

---

## 解题思路

这道题是目前遇到的最简单的，Alice和Bob分别自增一个数字，Alice开始，只能增加到N-1，看谁最后没法自增，Alice赢了返回true，Bob赢了返回false，时间复杂度是O(n)。

- 取余判断

  ```
	public static boolean divisorGame(int N) {
		int length = N - 1;
		int value = length % 2;
		return value == 1;
	}
  ```