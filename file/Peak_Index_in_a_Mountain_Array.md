## Peak Index in a Mountain Array

Let's call an array A a mountain if the following properties hold:

A.length >= 3
There exists some 0 < i < A.length - 1 such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]
Given an array that is definitely a mountain, return any i such that A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1].

- Example 1:

  Input: [0,1,0]

  Output: 1

- Example 2:

  Input: [0,2,1,0]

  Output: 1

- Note:
  - 3 <= A.length <= 10000
  - 0 <= A[i] <= 10^6
  - A is a mountain, as defined above.

---

## 解题思路：

这道题就相当简单了，从一个数组中找出最大值的索引。其中最需要注意的一点，题目中描述的数据结构类型和提示中，都明确表明了，数值分布是一个山峰，也就是说只有一个最大值，这点非常重要。

- 遍历 + map

  这是第一版的代码，无外乎就是遍历对比，然后把最大值存起来，时间复杂度是O(n)。
  
  ```
    public int peakIndexInMountainArray(int[] A) {
        HashMap<Integer,Integer> map = new HashMap<>();
        int temp = -1;
        for(int i=0;i<A.length;i++){
            if (temp<A[i]){
                temp = A[i];
                map.put(1,i);
            }
        }
        return map.get(1);
    }
  ```

- 直接遍历
  
  上面加个map的原因就是忽略掉了介绍中数值分布是一个山峰的条件，以为是乱序的，所以根据这个条件，直接一个简单for循环就可以了，所以合适的才是最好的，一定要分析所有的已知条件。

  ```
  for(int i = 0; i < nums.length-1; i++){
      if(nums[i] > nums[i+1]) {
          return i;
      }
  }
  return -1;
  ```

- 二分搜索
  
  因为本质上就是一个搜索，也就是一个遍历排序的问题。所以用二分搜索最快，时间复杂度是O(logN)

  ```
    public int peakIndexInMountainArray(int[] A) {
        int lo = 0, hi = A.length - 1;
        while (lo < hi) {
            int mi = lo + (hi - lo) / 2;
            if (A[mi] < A[mi + 1])
                lo = mi + 1;
            else
                hi = mi;
        }
        return lo;
    }
  ```
  