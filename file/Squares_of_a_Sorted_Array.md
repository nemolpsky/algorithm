## Squares of a Sorted Array

Given an array of integers A sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

- Example 1:

  Input: [-4,-1,0,3,10]

  Output: [0,1,9,16,100]

- Example 2:

  Input: [-7,-3,2,3,11]

  Output: [4,9,9,49,121]
 
- Note:
  - 1 <= A.length <= 10000
  - -10000 <= A[i] <= 10000
  - A is sorted in non-decreasing order.

---

## 解题思路 
这道题就比较有意思了，给定一个数组，包含负数、正数，按递增排序，也就是从负数到正数，最后要求计算所有元素的平方然后再从小到大递增排序。问题就在于负数乘以平方之后也是正数了，整个顺序就乱了。

- 先计算平方，再排序

  这种办法是最简单的了，也是最笨的，根据选择的排序方法可以稍稍加快点效率，但是就别想着O(n)了，这时间复杂度绝对是让你哭的。

  ```
  public static int[] sortedSquares(int[] A) {

      int[] arr = new int[A.length];

      // 遍历计算平方值
      for (int i = 0; i < A.length; i++) {
          arr[i] = A[i] * A[i];
      }

      // 排序
      for (int l = 0; l < arr.length; l++) {
          for (int k = l + 1; k < arr.length; k++) {
              if (arr[k] < arr[l]) {
                  int temp = arr[k];
                  arr[k] = arr[l];
                  arr[l] = temp;
              }
          }
      }

      return arr;
  }
  ```

- 两点对比法

  因为有负数的关系，而且是从负数到整数，从小到大排序的原因，所以当所有元素计算出平方值后一定是两端的值大，越到中间值越小。比如[-4, -1, 0, 3, 10]会变为[16, 2, 0, 9, 100]，所以如果想要排序的话只需要将两端的值进行比较，100>16，16>9，9>2，因为唯一不确定的就是不知道左右谁大，但是两端的值一定是它那一端最大的值，所以直接比较两端的值，就相当于每次都拿两端中最大的值比较大小，循环下来就是一个正确的排序了。因为只遍历了一次，所以时间复杂度是O(n)。

  ```
    public static int[] sortedSquares3(int[] A) {

        int[] arr = new int[A.length];

        // 左右点的起始位置
        int L = 0;
        int R = A.length-1;
        // 当前索引
        int index = A.length-1;

        // 因为长度可能是单数，所以判断左边小于等于右边就继续比较
        while (L <= R){
            // 左右点对应元素的平方值
            int valL = A[L] * A[L];
            int valR = A[R] * A[R];

            // 判断哪个点的值大就存入新数组的尾部，再对左右点其中一个的索引自减
            if (valL>valR){
                arr[index] = valL;
                L++;
            }else{
                R--;
                arr[index] = valR;
            }
            // 当前索引值自减
            index--;
        }

        return arr;
    }
  ```