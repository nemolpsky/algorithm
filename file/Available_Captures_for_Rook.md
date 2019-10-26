## Available Captures for Rook

On an 8 x 8 chessboard, there is one white rook.  There also may be empty squares, white bishops, and black pawns.  These are given as characters 'R', '.', 'B', and 'p' respectively. Uppercase characters represent white pieces, and lowercase characters represent black pieces.

The rook moves as in the rules of Chess: it chooses one of four cardinal directions (north, east, west, and south), then moves in that direction until it chooses to stop, reaches the edge of the board, or captures an opposite colored pawn by moving to the same square it occupies.  Also, rooks cannot move into the same square as other friendly bishops.

Return the number of pawns the rook can capture in one move.

 

- Example 1:

  ![rock1](https://github.com/nemolpsky/algorithm/raw/master/file/image/rook1.png)

  Input: [[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

  Output: 3

  Explanation: 

  In this example the rook is able to capture all the pawns.

- Example 2:

  ![rock2](https://github.com/nemolpsky/algorithm/raw/master/file/image/rook2.png)

  Input: [[".",".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

  Output: 0

  Explanation: 

  Bishops are blocking the rook to capture any pawn.

- Example 3:

  ![rock3](https://github.com/nemolpsky/algorithm/raw/master/file/image/rook3.png)

  Input: [[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]

  Output: 3

  Explanation: 

  The rook can capture the pawns at positions b5, d6 and f5.
 

- Note:

  - board.length == board[i].length == 8
  - board[i][j] is either 'R', '.', 'B', or 'p'
  - There is exactly one cell with board[i][j] == 'R'

---

## 解题思路
这道题也是很有意思，有个8X8的棋盘，棋盘上会有一个Rook旗子，还会有若干黑棋和白棋，会使用二维数组来表示位置，其中R就表示Rook，B表示白棋，p表示黑棋。规则是这样的，Rook只能直线走，遇到棋子就停下来，算出Rook可以吃掉多少个棋子。

- 双层遍历

  按规则的话其实只要关注Rook所在位置的上下左右，也就是以Rook为中心的十字路线。遇到白棋就直接停止，遇到黑棋数量加1也停下来。所以先双层遍历算出Rook的位置，然后分别遍历上下左右的行列即可，时间复杂度是O(n²)。

  ```
	public int numRookCaptures(char[][] board) {
		int rowLength = 8;
		int columnLength = 8;

		int count = 0;

		// R的位置
		int x = 0;
		int y = 0;

		// 遍历寻找R的位置
		for (int i = 0; i < columnLength; i++) {
			for (int l = 0; l < rowLength; l++) {
				if (board[i][l] == 'R') {
					x = i;
					y = l;
				}

			}
		}

		// R的右边
		for (int row = x; row < rowLength; row++) {
			if (board[row][y] == 'B') {
				break;
			}
			if (board[row][y] == 'p') {
				count++;
				break;
			}
		}

		// R的左边
		for (int row = x; row > 0; row--) {
			if (board[row][y] == 'B') {
				break;
			}
			if (board[row][y] == 'p') {
				count++;
				break;
			}
		}

		// R的上边
		for (int column = y; column > 0; column--) {
			if (board[x][column] == 'B') {
				break;
			}
			if (board[x][column] == 'p') {
				count++;
				break;
			}
		}

		// R的下边
		for (int column = y; column < columnLength; column++) {
			if (board[x][column] == 'B') {
				break;
			}
			if (board[x][column] == 'p') {
				count++;
				break;
			}
		}

		return count;
	}
  ```