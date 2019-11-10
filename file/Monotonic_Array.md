## Monotonic Array

An array is monotonic if it is either monotone increasing or monotone decreasing.

An array A is monotone increasing if for all i <= j, A[i] <= A[j].  An array A is monotone decreasing if for all i <= j, A[i] >= A[j].

Return true if and only if the given array A is monotonic.


- Example 1:

  Input: [1,2,2,3]

  Output: true

- Example 2:

  Input: [6,5,4,4]

  Output: true

- Example 3:

  Input: [1,3,2]

  Output: false

- Example 4:

  Input: [1,2,4,5]

  Output: true

- Example 5:

  Input: [1,1,1]

  Output: true
 

- Note:

  - 1 <= A.length <= 50000
  - -100000 <= A[i] <= 100000
---

## 解题思路

这道题目的意思是给定一个数组，判断它是不是递增或递减。

- 遍历对比大小

  递增就是后面的一定大于等于前面的，反之亦然，所以遍历对比当前元素和后面的元素，再和上个元素和当前元素的大小比较结果进行对比，如果有不一致就表示不是单调的，时间复杂度是O(n)。

  ```
    public static boolean isMonotonic(int[] A) {

        // 对比结果，0相等 -1小于 1大于
        int compareValue = 0;
        // 遍历，最后一个元素不需要检查
        for (int i = 0; i < A.length - 1; i++) {
            int value = Integer.compare(A[i], A[i + 1]);
            // 相等符合递增或递减，不需要判断
            if (value != 0) {
                // 与0的不比较，和上次大小结果对比
                if (compareValue != 0 && compareValue != value) {
                    return false;
                }
                // 保存本次对比大小结果
                compareValue = value;
            }
        }
        return true;
    }
  ```