## Number of Recent Calls

Write a class RecentCounter to count recent requests.

It has only one method: ping(int t), where t represents some time in milliseconds.

Return the number of pings that have been made from 3000 milliseconds ago until now.

Any ping with time in [t - 3000, t] will count, including the current ping.

It is guaranteed that every call to ping uses a strictly larger value of t than before.

 

- Example 1:

  Input: 

  inputs = ["RecentCounter","ping","ping","ping","ping"], 
  inputs = [[],[1],[100],[3001],[3002]]

  Output: [null,1,2,3,3]
 

- Note:

  - Each test case will have at most 10000 calls to ping.
  - Each test case will call ping with strictly increasing values of t.
  - Each call to ping will have 1 <= t <= 10^9.

---

## 解题思路

这道题目本身真的很简单，但是题目写的真的很模糊，意思是有一个RecentCounter类，专门用来按规则计算请求次数，其中ping()方法就是计算的方法，接受一个时间值，统计在当前时间戳3000毫秒前之内包括自己的数量，也就是上面的[t-3000,t]范围，比如例子中的3002秒，它的3000毫秒前就是2毫秒，所以包括他自己的话就有3个请求符合，分别是[[100],[3001],[3002]]。

- 队列

  很多人都用的是队列，确实很简单，因为题目说了请求的时间参数是升序的，所以删除了的元素，对后面的添加的也不会有影响，时间复杂度是O(n)。

  ```
  class RecentCounter {

    Queue<Integer> queue;

    public RecentCounter() {
        queue = new LinkedList<Integer>();
    }

    public int ping(int t) {
        queue.add(t);
        while (queue.peek()<t-3000){
            queue.poll();
        }
        return queue.size();
    }
  }
  ```

- 数组
  
  用数组和用队列一个样，原理都是差不多，时间复杂度也是O(n)。

  ```
  class RecentCounter {
    int [] arr;
    int start = 0, end = 0;
    
    public RecentCounter() {
        
        arr = new int[10000];
        
    }
    
    public int ping(int t) {
        
        arr[end++] = t;
        
        while(t - arr[start] > 3000){
            start++;
        }  
        return end-start;
    }
  }
  ```