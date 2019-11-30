## Flood Fill

An image is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).

Given a coordinate (sr, sc) representing the starting pixel (row and column) of the flood fill, and a pixel value newColor, "flood fill" the image.

To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.

At the end, return the modified image.

- Example 1:

  Input: 

  image = [[1,1,1],[1,1,0],[1,0,1]] sr = 1, sc = 1, newColor = 2

  Output: 
  
  [[2,2,2],[2,2,0],[2,0,1]]

  Explanation: 

  From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected by a path of the same color as the starting pixel are colored with the new color.Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.

- Note:

  - The length of image and image[0] will be in the range [1, 50].
  - The given starting pixel will satisfy 0 <= sr < image.length and 0 <= sc < image[0].length.
  - The value of each color in image[i][j] and newColor will be an integer in [0, 65535].
---

## 解题思路

这道题的意思写的云里雾里的，其实就是说给定一个多维数组，再给定任意一个坐标，然后将当前坐标值的上下左右四个点的值都修改成新值，前提是这些点的值和初始点的值一样，然后这四个点再改它们的上下左右，直到把所有符合规则的点都改完。

- 递归
  
  这道题其实就是从一个点开始像四周遍历，也就是深度优先搜索，只需要判断是否符合修改的条件和数组边界问题，时间复杂度是O(n)。

  ```

    public static int[][] floodFill(int[][] image, int sr, int sc, int newColor) {

        // 横列长度
        int raw = image.length;
        // 纵列长度
        int column = image[0].length;
        // 初始点的颜色
        int oldColor = image[sr][sc];
        // 从起始点开始遍历
        if (oldColor != newColor) {
            search(image, sr, sc, oldColor, newColor, raw, column);
        }

        return image;
    }

    public static void search(int[][] image, int x, int y, int oldColor, int newColor, int raw, int column) {
        // 该点的颜色和初始点颜色才修改
        if (image[x][y] == oldColor) {
            image[x][y] = newColor;
            // 上
            if (x > 0) {
                search(image, x - 1, y, oldColor, newColor, raw, column);
            }
            // 下
            if (x + 1 < raw) {
                search(image, x + 1, y, oldColor, newColor, raw, column);
            }
            // 左
            if (y > 0) {
                search(image, x, y - 1, oldColor, newColor, raw, column);
            }
            // 右
            if (y + 1 < column) {
                search(image, x, y + 1, oldColor, newColor, raw, column);
            }
        }
    }
  ```