## 广度优先搜索

广度优先搜索其实就是在一个复杂的关系图中找到两点之间所需步数最少的路径。

---

- 关系图

  比如下面着这张图就是就一张关系图，总共有6个点，每个点都可以到达其他不同的点，要注意的是这个图是单向图，即每个节点之间的连接都有箭头指向，还有双向图，比如CAB可以到CAT，CAT也可以到CAB，而不是像现在这样CAB可以到CAT，但是CAT不能到CAB。
  
  ![找出最短路径](https://github.com/nemolpsky/algorithm/raw/master/file/image/2.png)

- 原理
  
  比如在上面的关系图中CAB是起点，BAT是终点，所以需要找到两点之间步数最少的路径。

  其实原理并不难，既然CAB是起点，可以先筛选离CAB最近的节点，如果没有找到终点就继续筛选附近节点的附近节点，比如要找的节点是BAT，而CAB附近的节点是CAT和CAR，而不是BAT，所以就继续查找CAT和CAR附近的节点，刚好CAT的其中一个附近的节点就是BAT。也就是说走了两步就可以到达BAT，第一步是查找了CAT和CAT，第二步则是查找了MAT、BAR和BAT。这就是广度优先搜索的原理了，一层一层的由近到远的去搜索，直到找到终点为止。


- 代码实现

  - 使用散列表来表示节点的关系

    可以看到是建立了一个HashMap来存放每个节点和它的邻近节点的关系，然后将起始节点放入队列中，开始循环队列，从头部取出一个元素也就是起始节点，判断它的邻近节点是不是终点，如果不是则把邻近节点也都放入队列中，因为每次循环都是从队列头部获取，这样就保证了查找都是先查找起始节点的本身，没有则查找它的邻近节点，如果还没有则查找邻近节点的邻近节点。一层层的查下去，知道查到为止。

    ```
    public static int search() {
        // 存放节点，循环检查的队列
        LinkedList<String[]> linkedList = new LinkedList<>();

        // 使用map来存储每个节点和临近节点之间的关系
        HashMap<String, String[]> map = new HashMap<>();
        map.put("CAB", new String[]{"CAR", "CAT"});
        map.put("BAR", new String[]{"BAT"});
        map.put("CAT", new String[]{"MAT", "BAT"});
        map.put("CAR", new String[]{"BAR", "CAT"});
        map.put("MAT", new String[]{"BAT"});
        map.put("BAT", new String[]{});

        // 起点是CAB，将CAB的临近节点放到队列中
        linkedList.add(map.get("CAB"));

        // 循环队列
        while (!linkedList.isEmpty()) {
            // 存放下一层临近节点的队列
            LinkedList<String[]> newLinkedList = new LinkedList<>();
            // 检查队列中的每个节点的临近节点
            String[] arr = linkedList.poll();
            System.out.println("====");
            // 循环检查临近每个临近节点
            for (int i = 0; i < arr.length; i++) {
                System.out.println(arr[i]);
                // 判断如果该节点就是要找的节点就停止遍历
                if (arr[i].equals("BAT")) {
                    return arr[i];
                } else {
                    // 将该节点的临近节点也放入队列中
                    newLinkedList.add(map.get(arr[i]));
                }
            }

            // 使用存放下一层的队列来循环
            linkedList = newLinkedList;
        }

        return null;
    }

    ```
    输出结果
    ```
    ====
    CAR
    CAT
    ====
    BAR
    CAT
    ====
    BAT
    ```
   - 使用自定义Node节点类

     如果在Leetcode上刷过关于树的题一定会见过自定义节点类，这个问题也可以使用同样的自定义节点类，有节点名字和邻近节点的变量。至于查找的逻辑则跟上面一样。

     Node节点类
     ```
     class Node {
        public Node(String name, Node[] neighbours) {
            this.name = name;
            this.neighbours = neighbours;
        }

        String name;
        Node[] neighbours;

        @Override
        public String toString() {
            return "Node{" +
                "name='" + name + '\'' +
                ", neighbours=" + Arrays.toString(neighbours) +
                '}';
        }
     }
     ```

     查找逻辑
     ```
     public static Node search() {
        // 存放节点，循环检查的队列
        LinkedList<Node> linkedList = new LinkedList<>();

        // 初始化节点
        Node bat = new Node("BAT", null);
        Node mat = new Node("MAT", new Node[]{bat});
        Node cat = new Node("CAT", new Node[]{mat, bat});
        Node bar = new Node("BAR", new Node[]{bat});
        Node car = new Node("CAR", new Node[]{bar, cat});
        Node cab = new Node("CAB", new Node[]{car, cat});

        // 将起始节点添加到队列中
        linkedList.add(cab);

        // 循环队列
        while (!linkedList.isEmpty()) {
            // 存放下一层邻近节点的队列
            LinkedList<Node> newLinkedList = new LinkedList<>();
            // 获取一个队列顶层的节点
            Node nextNode = linkedList.poll();
            System.out.println("====");
            // 查看这个节点的邻近节点
            for (int i = 0; i < nextNode.neighbours.length; i++) {
                Node currentNode = nextNode.neighbours[i];
                System.out.println(currentNode.name);
                // 判断是否是要找的节点，如果是则返回
                if (currentNode.name.equals("BAT")) {
                    return currentNode;
                } else {
                    // 如果不是则放入队列中
                    newLinkedList.add(currentNode);
                }
            }

            // 循环存放下一层节点的队列
            linkedList = newLinkedList;
        }
        return null;
     }
     ```
     结果
     ```
     ====
     CAR
     CAT
     ====
     BAR
     CAT
     ====
     BAT
     ```
