## 狄克斯特拉算法(Dijkstra)

这个算法是在广度优先搜索的基础上增加了对权重的筛选，广度优先搜索是找出到达终点最少步数的路径，而狄克斯特拉算法则是找出到达终点距离最短的路径，注意就是最短距离，这就是指加权图，就是每个节点之间都有数值，需要找出到达终点所需数值最小的图，这个时候步数最少的就不一定是距离最短的。

---

- 加权图

  ![加权图](https://github.com/nemolpsky/algorithm/raw/master/file/image/dijkstra1.png)


- 原理

  - 从起点开始遍历到邻近节点，也就是一层节点，看哪个离哪个节点的距离最近。
  - 继续上面的步骤从找到的最近的邻近节点开始继续遍历寻找它的邻近节点，也就是二层节点，直到到达终点。

  因为是在广度优先搜索的基础上的算法，所以原理其实很相似。比如上面起点的邻近节点是A和B，而B是最近的，所以一层节点中最近的是B，而B的邻近节点则是A和D，D又是最近的，所以二层节点中最近的是D，D的邻近节点已经是终点了，所以也就找出了到达终点的最近路线。

  
- 代码实现

  首先也类似广度优先搜索算法一样，需要建立每个节点之间的关系，只不过在这个基础上还建立每个节点到它邻近节点之间的距离，这样才能对比距离的大小。可以看到起点到邻近点的距离我们是可以知道的，但是其他节点到邻近节点的距离是不知道的所以设置了一个极大值。还增加了一个map来存放每个节点的父级节点，相当于保存了到达每个节点的路线。

  可以看到初始化所有数据后，首先将起点作为参数调用getShortNode()方法来查找它的距离最近的邻近节点，查到是B，因为B有多个邻近节点，所以会遍历B的邻近节点，用起点到B点距离加上每个邻近节点的距离，看看哪个近，以此类的推，每次找到最近的都会更新存放距离和父节点的Map，以此类推，找到终点的时候，也就找到了一条距离最近的路线。

  ```
    public static int search() {

        // 存放检查过的集合
        List<String> checkList = new ArrayList<>();

        // 初始化所有节点的邻近节点和到邻近节点的距离
        HashMap<String, HashMap<String, Integer>> nodeMap = new HashMap<>();
        nodeMap.put("START", new HashMap<String, Integer>());
        nodeMap.get("START").put("A", 5);
        nodeMap.get("START").put("B", 2);

        nodeMap.put("A", new HashMap<String, Integer>());
        nodeMap.get("A").put("C", 4);
        nodeMap.get("A").put("D", 2);

        nodeMap.put("B", new HashMap<String, Integer>());
        nodeMap.get("B").put("A", 8);
        nodeMap.get("B").put("D", 7);

        nodeMap.put("C", new HashMap<String, Integer>());
        nodeMap.get("C").put("D", 6);
        nodeMap.get("C").put("END", 3);

        nodeMap.put("D", new HashMap<String, Integer>());
        nodeMap.get("D").put("END", 1);

        nodeMap.put("END", new HashMap<String, Integer>());

        // 保存每个节点的父节点，初始化起点
        HashMap<String, String> parent = new HashMap<>();
        parent.put("A", "START");
        parent.put("B", "START");
        parent.put("C", null);
        parent.put("D", null);
        parent.put("D", null);
        parent.put("END", null);

        // 保存到每个点的最近的距离，初始化起点附近的点
        HashMap<String, Integer> costMap = new HashMap<>();
        costMap.put("A", 5);
        costMap.put("B", 2);
        costMap.put("C", Integer.MAX_VALUE);
        costMap.put("D", Integer.MAX_VALUE);
        costMap.put("D", Integer.MAX_VALUE);
        costMap.put("END", Integer.MAX_VALUE);

        // 搜索起点到未遍历的点中所有距离最短的
        String node = getShortNode(costMap, checkList);

        // 队列中有节点就一直继续
        while (node != null) {
            // 获取到达该节点的距离
            int l = costMap.get(node);
            // 该节点的邻近节点
            HashMap<String, Integer> neighborsNode = nodeMap.get(node);
            // 遍历邻近所有节点
            for (String neighbor : neighborsNode.keySet()) {
                // 每次都拿到达当前节点的距离到加上附近节点的距离
                int length = l + neighborsNode.get(neighbor);
                // 遍历所有的邻近节点，拿上面相加的距离对比，如果找到更近的则更新map中的值和父节点
                if (costMap.get(neighbor) > length) {
                    // 添加到每个节点的最近距离
                    costMap.put(neighbor, length);
                    // 添加父节点
                    parent.put(neighbor, node);
                }
            }

            // 遍历完之后添加到遍历过的集合中
            checkList.add(node);

            // 继续在找到的这个最近节点的基础上查找下个最近的节点
            node = getShortNode(costMap, checkList);
        }

        System.out.println(parent);
        System.out.println(costMap);
        return costMap.get("END");
    }

    public static String getShortNode(HashMap<String, Integer> map, List<String> processed) {
        // 距离最近的点距离，初始值是极大值
        Integer shortLength = Integer.MAX_VALUE;
        // 距离最近的节点
        String shortNode = null;
        // 遍历所有节点寻找距离最近的点
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            // 过滤掉已经走过的点，并且找到最近的点
            if (entry.getValue() < shortLength && !processed.contains(entry.getKey())) {
                shortLength = entry.getValue();
                shortNode = entry.getKey();
            }
        }
        // 返回这个节点
        return shortNode;
    }
  ```
  
  输出结果

  可以看到是把达到所有节点的最近的距离都算出来了，根据保存的父类节点也可以看出最短的路线。
 
  ```
  {A=START, B=START, C=A, D=A, END=D}
  {A=5, B=2, C=9, D=7, END=8}
  ```

- 注意
  
  - 狄克斯特拉算法不适用于负权边的加权图，负权边的意思是一个节点达到另一个节点不仅不需要增加距离，反而可以减少。因为是每次都查找最短距离的邻近节点，所以有可能选择了一个距离最小的邻近节点，却发现如果选择另一个节点的话就可以选择一个负权边的节点，但是因为选择距离最小的规则而错过了这个负权边的节点，比如下图。

    ![负加权图](https://github.com/nemolpsky/algorithm/raw/master/file/image/dijkstra2.png)

  - 还有一种情况不适用，就是有环的加权图，比如下面这个，A会先找到B，然后B会找到C，因为C比D进，然后D又找到B，B再找到C。所以按照一直寻找距离最近的节点的规则是无法找到最近的距离的，更何况还过滤掉了已经检查过的节点，循环根本就走不下。

    ![有环加权图](https://github.com/nemolpsky/algorithm/raw/master/file/image/dijkstra3.png)