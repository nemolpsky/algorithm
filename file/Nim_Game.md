## Nim Game

You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

- Example:

  Input: 4

  Output: false 

  Explanation: 

  If there are 4 stones in the heap, then you will never win the game;No matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

--- 
## 解题思路

这道题是一道纯粹的数学题，你和朋友分石头，你是先手，每次可以分[1,3]，谁拿到最后的石头谁就赢。

- 数学归纳法推导

  ```
  1. 在4个的情况下你必输，因为无论你分多少，对手都可以拿到最后一个
  2. 在8个的情况下也是，无论你拿多少最后都可以剩下4个，然后按条件1就输了
  3. 假设有n个石头，是4的倍数4n，推导一定可以剩下4个，也就是4n-4
  4. 假设每次一个人拿x个，另一个人拿4-x，就是4n-x-(4-x)，最后简化为4n-4
  5. 所有也就证明了，只要n是4倍的倍数，一定能剩4个，也就是必输。
  ```
  
  ```
    public boolean canWinNim(int n) {
        return n % 4 != 0;
    }
  ```