## Projection Area of 3D Shapes

On a N * N grid, we place some 1 * 1 * 1 cubes that are axis-aligned with the x, y, and z axes.

Each value v = grid[i][j] represents a tower of v cubes placed on top of grid cell (i, j).

Now we view the projection of these cubes onto the xy, yz, and zx planes.

A projection is like a shadow, that maps our 3 dimensional figure to a 2 dimensional plane. 

Here, we are viewing the "shadow" when looking at the cubes from the top, the front, and the side.

Return the total area of all three projections.

 

- Example 1:

  Input: [[2]]

  Output: 5

- Example 2:

  Input: [[1,2],[3,4]]

  Output: 17

  Explanation: 

  Here are the three projections ("shadows") of the shape made with each axis-aligned plane.

  ![三维图](https://github.com/nemolpsky/algorithm/raw/master/file/image/shadow.png)
  

- Example 3:

  Input: [[1,0],[0,2]]

  Output: 8

- Example 4:

  Input: [[1,1,1],[1,0,1],[1,1,1]]

  Output: 14

- Example 5:

  Input: [[2,2,2],[2,1,2],[2,2,2]]

  Output: 21
 

- Note:

  - 1 <= grid.length = grid[0].length <= 50
  - 0 <= grid[i][j] <= 50

---

## 解题思路

这道题目就很有意思，给定了一个二维数字，这一个二维数组定义了一个三维图。注意看上面例子2中的图，可以发现，XY轴是俯视图，YZ图就是正视图也就是子数组依次摆放的顺序，而XZ则是侧视图。因为这三个视图下去看都会得到一个平面图，所以题目要求计算出三个视图中阴影面积的大小。

- 双层遍历

  因为每个视图看到的其实都是最大的那个长度的阴影，所以其实就是按不同的规则去获取最大值，比如俯视图，说白了就是看有几个方块，所以统计所有不为0的元素就可以，正视图，就是看所有子数组中同一个位置上哪个方块更长，侧视图则是看同一个子数组中哪个位置上的方块更长，了解这三个规则就比较容易解决了。

  ```
    public static int projectionArea(int[][] grid) {
        int count = 0;
        // xy轴是俯视图，也就是所有数组不为0的长度
        for (int[] child : grid) {
            // 过滤掉0
            count +=Arrays.stream(child).filter(c->c!=0).count();
        }

        // zx轴是侧视图，也就是比较每个子数组最大的值
        int[] zxArr = new int[grid.length];
        int zxArrIndex = 0;
        // 找出每个子数组中最大的元素，存入集合
        for (int[] child : grid) {
            int zxMax = 0;
            for (int c : child) {
                if (c > zxMax) {
                    zxMax = c;
                }
            }
            zxArr[zxArrIndex++] = zxMax;
        }


        // zy轴是正视图，也就是所有子数组中同索引位置上最大的
        int[] zyArr = new int[grid[0].length];
        // 遍历所有数组，对比所有数组同一个位置上的大小，将最大值存入数组
        for (int[] child : grid) {
            for (int i = 0; i < child.length; i++) {
                if (zyArr[i] < child[i]) {
                    zyArr[i] = child[i];
                }
            }
        }
        
        // 将两个存储最大值的数组里的值相加
        count += Arrays.stream(zxArr).reduce(0, Integer::sum);
        count += Arrays.stream(zyArr).reduce(0, Integer::sum);
        return count;
    }
  ```
  
- 优化双层遍历

  上面的是最原始的代码，为了逻辑更清晰将三个视图的计算都分开双层遍历，但是既然都是在双层遍历，肯定是可以合在一起的，下面就是根据上面的代码优化的，其实还可以更精简优化。

  ```
    public static int projectionArea1(int[][] grid) {
        int count = 0;

        int[] zyArr = new int[grid[0].length];

        for(int i=0;i<grid.length;i++){
            // 每个子数组最大的值
            int zxMax = 0;

            // xy轴是俯视图，也就是所有数组不为0的长度
            for(int l=0;l<grid[i].length;l++){
                // 过滤掉0
                if (grid[i][l]!=0){
                    count++;
                }

                // zx轴是侧视图，也就是比较每个子数组最大的值
                if (grid[i][l]>zxMax){
                    zxMax= grid[i][l];
                }

                // zy轴是正视图，也就是所有子数组中同索引位置上最大的
                if (grid[i][l]>zyArr[l]){
                    // 遍历所有数组，对比所有数组同一个位置上的大小，将最大值存入数组
                    zyArr[l]=grid[i][l];
                }
            }
            count+=zxMax;
        }

        // 将存储最大值的数组里的值相加
        for(int c:zyArr){
            count += c;
        }
        return count;
    }
  ```