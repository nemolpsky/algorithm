## Robot Return to Origin

There is a robot starting at position (0, 0), the origin, on a 2D plane. Given a sequence of its moves, judge if this robot ends up at (0, 0) after it completes its moves.

The move sequence is represented by a string, and the character moves[i] represents its ith move. Valid moves are R (right), L (left), U (up), and D (down). If the robot returns to the origin after it finishes all of its moves, return true. Otherwise, return false.

Note: The way that the robot is "facing" is irrelevant. "R" will always make the robot move to the right once, "L" will always make it move left, etc. Also, assume that the magnitude of the robot's movement is the same for each move.

- Example 1:

  Input: "UD"

  Output: true 

  Explanation: The robot moves up once, and then down once. All moves have the same magnitude, so it ended up at the origin where it started. Therefore, we return true.
 

- Example 2:

  Input: "LL"
  
  Output: false

  Explanation: The robot moves left twice. It ends up two "moves" to the left of the origin. We return false because it is not at the origin at the end of its moves.

---

## 解题思路
这道题的描述看上去很长，但是意思很简单，假设一个机器人的起点是(0,0)坐标，接受LRUP四个字母来移动，分标达标左右上下，假设移动的距离和方向都不考虑，给定一个字符串包含这四个字母，如果移动完之后能够回到原点就返回true，否则返回false，其实就像是推箱子，往左推一下再往右一下就会回到终点，往左一下往右两下就回不到原点。

- 上下抵消、左右抵消

  思路其实很简单，如果上下移动的次数一样且左右移动的次数也一样就可以回到原点，定义两个变量分别记录横向和纵向移动即可

  ```
  public static boolean judgeCircle(String moves) {
      // 记录横向的值
	  int horizontal = 0;
      // 记录纵向的值
	  int vertical = 0;

      // 遍历字符串
	  for (char c : moves.toCharArray()) {
		  if (c == 'L') { // 左-1
			  horizontal--;
		  }else if (c == 'R') { // 右+1
			  horizontal++;
		  }else if (c == 'U') { // 上+1
			  vertical++;
		  }else if (c == 'D') { // 下-1
			  vertical--;
		  }
	  }

      // 判断是否都为0
	  return horizontal == 0 && vertical == 0;
  }
  ```