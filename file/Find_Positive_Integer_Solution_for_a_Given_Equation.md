## 

Given a function  f(x, y) and a value z, return all positive integer pairs x and y where f(x,y) == z.

The function is constantly increasing, i.e.:

f(x, y) < f(x + 1, y)
f(x, y) < f(x, y + 1)
The function interface is defined like this: 
```
interface CustomFunction {
public:
  // Returns positive integer f(x, y) for any given positive integer x and y.
  int f(int x, int y);
};
```
For custom testing purposes you're given an integer function_id and a target z as input, where function_id represent one function from an secret internal list, on the examples you'll know only two functions from the list.  

You may return the solutions in any order.

 

- Example 1:

  Input: function_id = 1, z = 5

  Output: [[1,4],[2,3],[3,2],[4,1]]

  Explanation: function_id = 1 means that f(x, y) = x + y

- Example 2:

  Input: function_id = 2, z = 5

  Output: [[1,5],[5,1]]

  Explanation: function_id = 2 means that f(x, y) = x * y
 

- Constraints:

  - 1 <= function_id <= 9
  - 1 <= z <= 100
  - It's guaranteed that the solutions of f(x, y) == z will be on the range 1 <= x, y <= 1000
  - It's also guaranteed that f(x, y) will fit in 32 bit signed integer if 1 <= x, y <= 1000
---

## 解题思路

这道题的出题形式比较奇怪，比较让人难看懂题目是啥意思，其实就是说给定了一个接口CustomFunction，里面定义了9个方法，分别是对x和y值的不同计算方式，会得出一个结果，需要做的是把所有结果等于z的x、y值都返回，此外这个函数是单调增长的，f(x, y) < f(x + 1, y)，f(x, y) < f(x, y + 1)，1 <= x, y <= 1000，说白了就是要列出所有函数中解等于z值的x、y值，根本不需要关系函数里面定义的啥。

- 双层遍历

  只需要把每个x、y组合遍历看和z是否相同就可以，同时因为说了是f(x, y) < f(x + 1, y)，f(x, y) < f(x, y + 1)，所以一旦值比z大后面的也就不需要再检查了，一定比z大，时间复杂度是O(n)。

  ```
    public List<List<Integer>> findSolution(CustomFunction f, int z) {
        List<List<Integer>> list = new ArrayList<>();

        for (int x = 1; x < 1000; x++) {
            for (int y = 1; y < 1000; y++) {
                if (f.f(x, y) > z) {
                    // 因为是递增的，所以如果本次大于z后面的也不需要检查
                    break;
                }

                if (f.f(x, y) == z) {
                    // 如果等于就放入
                    list.add(Arrays.asList(new Integer[]{x, y}));
                }
            }
        }

        return list;
    }
  ```

- 双指针

  因为f(x, y) < f(x + 1, y)，f(x, y) < f(x, y + 1)是单调递增的，所以其实不需要判断x,y值到[1,1000]，只需要到[1,z]即可，下面是在LeetCode下的一个答案的里的推导过程，解释了原因。

  ```
  //    由函数的单调可知，对于任意x,y属于区间[1, 1000]，
  //    存在f(0, y) < f(0 + x, y)
  //    令f(0, y) == 1，即取最小值，
  //    考虑单调性f(1, y) > f(0, y) == 1，
  //    由于接口的整形特征可得f(1, y) >= 2，
  //    同理归纳可得f(i, y) >= i + 1，
  //    令 i == z，则有f(z, y) >= z + 1
  //    同理可得f(x, z) >= z + 1
  //    即当x >= z 或 y >= z时，有f(x, y) >= z + 1 > z
  //    所以查找的时候x,y只用考虑在[1,z]的范围里就可以
  //    遍历[1, 1000] * [1, 1000]完全多余。
    public List<List<Integer>> findSolution1(CustomFunction f, int z) {
        List<List<Integer>> list = new ArrayList<>();

        // 初始化x、y点
        int x = 1;
        int y = z;
        while (x <= z && y >= 1) {

            int value = f.f(x, y);
            if (value == z) {
                // 因为y是从1000开始，所以如果不等于的话，下面所有的值都会小于z
                list.add(Arrays.asList(new Integer[]{x, y}));
                x++;
            } else if (value > z) {
                // 如果比z大就减少y
                y--;
            } else {
                // 如果比z小就增大x
                x++;
            }
        }

        return list;
    }
  ```

  