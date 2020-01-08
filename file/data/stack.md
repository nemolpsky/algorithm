## 堆栈

堆栈在计算机科学中运用的很广，很多系统或者是编程语言都内部都运用了这种数据结构，所以其实很多语言中的递归实际上就是一种对堆栈的应用，它是一种先进后出(FILO)的模式。

---

### 数据结构

堆栈，也被称为栈，底层的数据结构其实是一种特殊的线性表结构，线性表结构的特点就是元素之间是相互连接的，每个元素都有一个前置元素，除了头部元素之外，每个元素都有一个后置元素，除了尾部元素之外。只不过线性表是允许在任意位置进行元素的插入和删除，而栈则不行，栈遵循的原则是先进后出(FILO)，可以想象想忘一个抽屉里放文件，每次只放张，只能放在上次放的文件的上面，而取的时候也每次只能取一张，只能取最上面的，这就是栈。

![堆栈](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/stack.png)

---
### Java实现简易栈

先是定义好节点的对象结构，有两个属性，分别是存储的值和前置节点。然后就是每次入栈的时候都要创建一个新节点来保存值，然后帮上次创建的节点作为当前节点的前置节点，出栈的时候则需要将当前节点弹出，当前节点的前置节点作为最顶层的节点。

    ```
    public class SimpleStack<T> {

    // 当前节点
    private Node currentNode;
    // 前置节点
    private Node pre;
    // 设置长度
    private int length;
    // 长度
    private int size;

    public SimpleStack() {
        this.length = -1;
    }

    // 可以限定栈的操作
    public SimpleStack(int length) {
        this.length = length;
    }

    // 入栈操作
    public void push(T value){
        if (length>0 && size>= length){
            return ;
        }

        // 累计长度
        count(true);

        // 第一次添加的时候
        if (currentNode==null) {
            // 创建当前节点，填入数值
            currentNode = new Node();
            currentNode.value = value;
        }
        // 第一次之后的操作
        else{
            // 当前节点变为前置节点
            pre = currentNode;
            // 重新创建一个当前节点，关联前置节点
            currentNode = new Node();
            currentNode.pre = pre;
            // 设置值
            currentNode.value = value;
        }
    }

    // 弹出栈顶元素
    public T pop(){
        if (currentNode==null) {
            return null;
        }

        // 减少长度
        count(false);

        T value = (T) currentNode.value;
        currentNode = currentNode.pre;
        return value;
    }

    // 对栈长度的计算
    public void count(boolean isAdd){
        if (isAdd){
            size++;
        }else if (!isAdd && size>0){
            size--;
        }
    }


    // 获取栈的长度
    public int size(){
        return size;
    }

    // 判断栈是否为空
    public boolean isEmpty(){
        return size==0;
    }

    // 重写toString方法，打印栈中内容
    @Override
    public String toString() {
        if (currentNode==null){
            return "[]";
        }
        Node node = currentNode;
        StringBuilder builder = new StringBuilder();
        builder.append("[");
        while (node!=null){
            builder.append(node.value).append(",");
            node = node.pre;
        }
        builder.deleteCharAt(builder.length()-1);
        builder.append("]");
        return builder.toString();
    }

    }

    // 节点对象，存的值和前置节点
    class Node<T> {
     T value;
     Node<T> pre;
    }
    ```

    测试结果，可以看到确实是先进后出的顺序。

    ```
    public static void main(String[] args){
        SimpleStack<String> stack = new SimpleStack<String>();

        for(int i=0;i<10;i++){
            stack.push(i+"str");
        }

        System.out.println("栈长度-" + stack.size());
        System.out.println("栈是否为空-" + stack.isEmpty());

        for(int i =0;i<10;i++){
            System.out.println(stack.pop());
        }
        System.out.println("栈长度-" + stack.size());
        System.out.println("栈是否为空-" + stack.isEmpty());
    }

    栈长度-10
    栈是否为空-false
    9str
    8str
    7str
    6str
    5str
    4str
    3str
    2str
    1str
    0str
    栈长度-0
    栈是否为空-true
    ```

---
### Java中自带的栈

Java中本身就已经有实现的Stack栈了，Stack继承自Vector，相比之下自带的栈功能就更加丰富。

   ```
   Stack<Integer> stack = new Stack<Integer>();
   stack.add(1);
   stack.push(1);
   stack.add(1,2);
   stack.addAll(new ArrayList<>());
   ```

---

### DFS
DFS是Depth-First Search的简称，也就是深度优先搜索，其实跟广度优先搜索很像，区别在于广度优先搜索是一层一层从一个点逐步扩散搜索，而深度优先搜索则是找到节点其中一个方向，直接遍历到该方向的底部，再回溯到该节点的继续遍历另外方向的底部，直到找到目标位置，所以它的查找方式和广度优先搜索不一样，查找出来的也很有可能不是最短路径。

比如下面这个例子，从A节点开始，一直遍历到E节点，遍历的时候A、B和E都入栈了，到达底部后再把BE出栈。

![堆栈](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/stack1.png)

![堆栈](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/stack2.png)

再回到A节点，把C、F和G节点入栈，找到G节点，完成搜索了，但是可以看到这不是最短路径。

![堆栈](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/stack3.png)

![堆栈](https://github.com/nemolpsky/algorithm/raw/master/file/data/image/stack4.png)


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

  因为只需要判断左上右下是否有连接的1，所以使用深度优先搜索，找出整块岛屿进行标记，然后再回溯查找另外的岛屿即可，时间复杂度是O(n)，n=二维数组的长*二维数组的宽。

  - 递归

    ```
    int column = 0;
    int row = 0;

    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        // 长度
        column = grid.length;
        row = grid[0].length;

        int num = 0;

        // 双层遍历
        for (int i = 0; i < column; i++) {
            for (int l = 0; l < row; l++) {
                // 如果为1则放入队列
                char c = grid[i][l];
                if (c == '1') {
                    grid[i][l] = '0';
                    num++;
                    dfs(i,l,grid);
                }
            }
        }
        return num;
    }

    public void dfs(int x,int y,char[][] grid){
        if (y-1 >= 0 && grid[x][y - 1] == '1') {
            grid[x][y - 1] = '0';
            // 继续查找该节点的左上右下
            dfs(x,y-1,grid);
        }
        //上
        if (x-1 >= 0 && grid[x - 1][y] == '1') {
            grid[x - 1][y] = '0';
            // 继续查找该节点的左上右下
            dfs(x-1,y,grid);
        }
        //右
        if (y+1 < row && grid[x][y + 1] == '1') {
            grid[x][y + 1] = '0';
            // 继续查找该节点的左上右下
            dfs(x,y+1,grid);
        }
        //下
        if (x+1 < column && grid[x + 1][y] == '1') {
            grid[x + 1][y] = '0';
            // 继续查找该节点的左上右下
            dfs(x+1,y,grid);
        }
    }
    ```

   - 堆栈

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
                // 栈，存储每个位置的坐标
                Stack<Position> stack = new Stack<Position>();
                char c = grid[i][l];
                // 如果为1则放入堆栈
                if (c == '1') {
                    stack.add(new Position(i, l));
                    grid[i][l] = '0';
                    num++;
                }

                // 堆栈不为空就一直执行，这样就会把连续的岛屿全部标记上
                while (!stack.isEmpty()) {
                    // 依次取出坐标位置
                    Position position = stack.pop();
                    int x = position.x;
                    int y = position.y;
                    // 下面是分别判断当前左边的上下左右是不是岛屿，如果是则标记上
                    // 并且把坐标放入堆栈，也就是继续判断它的上下左右的坐标
                    //左
                    if (y-1 >= 0 && grid[x][y - 1] == '1') {
                        stack.add(new Position(x, y - 1));
                        grid[x][y - 1] = '0';
                    }
                    //上
                    if (x-1 >= 0 && grid[x - 1][y] == '1') {
                        stack.add(new Position(x - 1, y));
                        grid[x - 1][y] = '0';
                    }
                    //右
                    if (y+1 < row && grid[x][y + 1] == '1') {
                        stack.add(new Position(x, y + 1));
                        grid[x][y + 1] = '0';
                    }
                    //下
                    if (x+1 < column && grid[x + 1][y] == '1') {
                        stack.add(new Position(x + 1, y));
                        grid[x + 1][y] = '0';
                    }
                }
            }
        }
        return num;
     }
     ```