## 队列

队列是一种非常常用的数据结构，它是先进先出(FIFO)的模式，每次添加元素都是添加在队列的尾部，而移除元素都是在将队列第一个移除出去，是线性数据结构。

---


### 添加和移除

既然队列每次添加和移除都是分别操作队列的头部和尾部元素，所以肯定就需要两个指针来指向当前最新的头部元素和尾部元素，然后随着添加和移除操作来对指针进行移动。

- 添加
      
  下面这张图就是添加操作，可以看到原本队列中有四个元素，尾部指针应该指向2，然后又向尾部添加了一个10，那尾部指针应该指向10。

  ![队列](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/queue1.png)
    
- 下面这张图是移除操作，原本头部是5，移除之后头部指针就指向5后面的13了。

  ![队列](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/queue2.png)

---

### Java实现简易循环队列
    
下面是实现的一个简易循环队列，内部是一个固定长度的数组，所以头元素和尾元素会形成一个环状结构。

```
    public class MyCircularQueue {

        private Integer[] arr = null;
        private int front;
        private int rear;
        private int startIndex = 0;
        private int endIndex = 0;
        private int length = 0;
        private int size = 0;

        /**
         * 初始化队列长度
         */
        public MyCircularQueue(int k) {
            arr = new Integer[k];
            length = k;
        }

        /**
         * 入队操作
         */
        public boolean enQueue(int value) {
            // 队列满了返回false
            if (isFull()) {
                return false;
            }
            // 更新尾部元素变量
            rear = value;
            arr[endIndex] = value;
            // 超出长度从0索引开始放置
            if (endIndex == length - 1) {
                endIndex = 0;
            } else {
                endIndex++;
            }
            size++;
            // 只有一个元素的时候头元素和尾元素是同一个元素
            if (size==1){
                front = rear;
            }

            return true;
        }

        /**
         * 出队操作
         */
        public boolean deQueue() {
            // 队列空的返回false
            if (isEmpty()) {
                return false;
            }

            // 对应位置置空
            arr[startIndex] = null;
            if (startIndex == length - 1) {
                startIndex = 0;
            } else {
                startIndex++;
            }
            size--;
            // 更新头元素变量
            if (size!=0) {
                front = arr[startIndex];
            }
            return true;
        }

        /**
         * 获取头部元素
         */
        public int Front() {
            return size!=0?front:-1;
        }

        /**
         * 获取尾部元素
         */
        public int Rear() {
            return size!=0?rear:-1;
        }

        /**
         *  判断队列是否为空
         */
        public boolean isEmpty() {
            return size == 0;
        }

        /**
         * 判断队列是否满了
         */
        public boolean isFull() {
            return size==length;
        }

    }
```

在上面那个版本的基础上还可以优化下，比如说下面的改进使用了求余的方法来判断位置，省去了循环队列中判断何时从头开始循环的麻烦，而且因为是使用两个指针来达成循环，只要对两个指针的位置进行控制就可以。

```
    class MyCircularQueue {

        private int[] data;
        private int head;
        private int tail;
        private int size;

        /**
         *  初始化长度，和头尾指针
         */
        public MyCircularQueue(int k) {
            data = new int[k];
            head = -1;
            tail = -1;
            size = k;
        }

        /**
         *  入队操作
         */
        public boolean enQueue(int value) {
            if (isFull()) {
                return false;
            }
        
            // 队列是空的，添加第一个元素，头指针指向索引0，添加操作只有第一次才会改变头指针的位置
            if (isEmpty()) {
                head = 0;
            }
        
            // 求余操作，适合计算循环操作的位置，+1是因为索引是从0开始
            tail = (tail + 1) % size;
            // 放入数组
            data[tail] = value;
            return true;
        }

        /**
         *  出队操作
         */
        public boolean deQueue() {
            if (isEmpty()) {
                return false;
            }
        
            // 头指针和尾指针在相同位置，表示队列中只有一个元素，出队后队列空了，需要重置两个指针的位置
            if (head == tail) {
                head = -1;
                tail = -1;
                return true;
            }
            // 头指针向后移动
            head = (head + 1) % size;
            return true;
        }

        /**
         *  获取头部元素
         */
        public int Front() {
            if (isEmpty()) {
                return -1;
            }
            return data[head];
        }

        /**
         *  获取尾部元素
         */
        public int Rear() {
            if (isEmpty() == true) {
                return -1;
            }
            return data[tail];
        }

        /**
         *  判断队列是否为空
         */
        public boolean isEmpty() {
            return head == -1;
        }

        /**
         *  判断队列是否满了
         */
        public boolean isFull() {
            return ((tail + 1) % size) == head;
        }
    }
```
---

### Java自带的队列实现

Java中其实也有提供已经实现好的队列，种类很多，```LinkedList```就是一个，它既实现了```List```接口又实现了```Deque```接口，所以它既是个集合也可以当做队列来使用，其他还有很多功能强大的队列。

```
    public class Main {
        public static void main(String[] args) {
            // 1. Initialize a queue.
            Queue<Integer> q = new LinkedList();
            // 2. Get the first element - return null if queue is empty.
            System.out.println("The first element is: " + q.peek());
            // 3. Push new element.
            q.offer(5);
            q.offer(13);
            q.offer(8);
            q.offer(6);
            // 4. Pop an element.
            q.poll();
            // 5. Get the first element.
            System.out.println("The first element is: " + q.peek());
            // 7. Get the size of the queue.
            System.out.println("The size is: " + q.size());
        }
    }
```
---

### BFS

BFS是Breadth-first Search的简称，也就是广度优先搜索，广度优先搜索一般是用于寻找最短路径所用的一种算法，比如下面这个例子就配合队列应用了广度优先搜索法。

这个例子是需要找到G节点的最短距离，根节点是A，所以步骤是第一轮先从A节点开始搜索，也就是B、C、D节点，没有找到G节点，把B、C、D节点放入队列。

![队列](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/queue3.png)

第二轮从队列取出B、C节点，把B、C、D节点的邻近节点也就是E、F、G节点一次放入队列，最后依次出队，因为D节点连接着G节点，所以G节点出队查找它的邻近节点也会找到G节点，也就是最短距离，只要2步就可以找到，所以就是依赖队列的先进先出(FIFO)保证从近到远搜索的顺序。

![队列](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/queue4.png)

---

### BFS算法题(Number of Islands)

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

- Example 1:

  Input:
  ```
  11110
  11010
  11000
  00000
  ```

  Output: 1

- Example 2:
  ```
  Input:
  11000
  11000
  00100
  00011
  ```
  Output: 3

- 解题思路

  这道题目的意思是给定一个二维数组来当做一块由岛屿和水组成的地方，1表示岛屿0表示水，如果1横竖连着的就算作同一块岛屿，要计算出总共有多少块岛屿。

  这道题目的难点在于如何判断岛屿是不是同一块，并且从何时开始寻找新的岛屿，这就很适合用BFS算法，因为BFS实际上是一种按顺序的遍历，所以首先是左上角开始循环，找到第一块岛屿的时候就利用队列的性质使用BFS找出这块岛屿的其他位置，全部标记成0，然后接着循环，等再遇到一块岛屿重复上述步骤，每次遇到一块岛屿就数量加1，这样就可以在不规则的相连的岛屿情况下算出有多少块岛屿了，时间复杂度是O(n)，n=二维数组的长*二维数组的宽。

  ```
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        // 长度
        int column = grid.length;
        int row = grid[0].length;

        // 数量
        int num = 0;

        // 双层遍历
        for (int i = 0; i < column; i++) {
            for (int l = 0; l < row; l++) {
                // 队列，存储每个位置的坐标
                LinkedList<Position> queue = new LinkedList<Position>();
                char c = grid[i][l];
                // 如果为1则放入队列
                if (c == '1') {
                    queue.add(new Position(i, l));
                    grid[i][l] = '0';
                    num++;
                }

                // 队列不为空就一直执行，这样就会把连续的岛屿全部标记上
                while (!queue.isEmpty()) {
                    // 依次取出坐标位置
                    Position position = queue.poll();
                    int x = position.x;
                    int y = position.y;
                    // 下面是分别判断当前左边的上下左右是不是岛屿，如果是则标记上
                    // 并且把坐标放入队列，也就是继续判断它的上下左右的坐标
                    //左
                    if (y-1 >= 0 && grid[x][y - 1] == '1') {
                        queue.add(new Position(x, y - 1));
                        grid[x][y - 1] = '0';
                    }
                    //上
                    if (x-1 >= 0 && grid[x - 1][y] == '1') {
                        queue.add(new Position(x - 1, y));
                        grid[x - 1][y] = '0';
                    }
                    //右
                    if (y+1 < row && grid[x][y + 1] == '1') {
                        queue.add(new Position(x, y + 1));
                        grid[x][y + 1] = '0';
                    }
                    //下
                    if (x+1 < column && grid[x + 1][y] == '1') {
                        queue.add(new Position(x + 1, y));
                        grid[x + 1][y] = '0';
                    }
                }
            }
        }
        return num;
    }
  ```
  

---

## 总结

- 队列是底层结构是线性结构，是按照先进先出(FIFO)的顺序输出数据。
- 循环队列其实就是依靠两个指针来在固定长度的线性表上达成一种循环，提高对空间的利用。