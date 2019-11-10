## Distance Between Bus Stops


A bus has n stops numbered from 0 to n - 1 that form a circle. We know the distance between all pairs of neighboring stops where distance[i] is the distance between the stops number i and (i + 1) % n.

The bus goes along both directions i.e. clockwise and counterclockwise.

Return the shortest distance between the given start and destination stops.

 

- Example 1:

  ![bus](https://github.com/nemolpsky/algorithm/raw/master/file/image/bus_stop_distance1.jpg)

  Input: distance = [1,2,3,4], start = 0, destination = 1

  Output: 1

  Explanation: Distance between 0 and 1 is 1 or 9, minimum is 1.
 

- Example 2:

  ![bus](https://github.com/nemolpsky/algorithm/raw/master/file/image/bus_stop_distance2.jpg)

  Input: distance = [1,2,3,4], start = 0, destination = 2

  Output: 3

  Explanation: Distance between 0 and 2 is 3 or 7, minimum is 3.
 

- Example 3:

  ![bus](https://github.com/nemolpsky/algorithm/raw/master/file/image/bus_stop_distance3.jpg)

  Input: distance = [1,2,3,4], start = 0, destination = 3

  Output: 4

  Explanation: Distance between 0 and 3 is 6 or 4, minimum is 4.

---

## 解题思路

这道题是说给定一个数组，里面包含每个站点到下一个站点的距离，刚好是首尾相连形成一个圈，找出一个点到另一个点的最近距离。

- 遍历

  这有点像是深度优先搜索，只不过结果不是树，而是一个环形，分别计算两段距离，然后对比哪一段更短，时间复杂度是O(n)。

  ```
    public static int distanceBetweenBusStops(int[] distance, int start, int destination) {
        // 如果起始点大于重点，就互换
        if (start > destination) {
            int temp = start;
            start = destination;
            destination = temp;
        }

        // 计算起点到终点的距离，比如[0,6]，2->5
        int length1 = 0;
        for (int i = start; i < destination; i++) {
            length1 += distance[i];
        }

        // 计算从反方向的距离，比如[0,6] , 0->2 + 5->6
        int length2 = 0;
        for (int i = 0; i < start; i++) {
            length2 += distance[i];
        }

        for (int i = destination; i < distance.length; i++) {
            length2 += distance[i];
        }

        // 对比两个方向的距离哪个短
        return Math.min(length1, length2);
    }
  ```


