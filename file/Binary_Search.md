## Binary Search

Given a sorted (in ascending order) integer array nums of n elements and a target value, write a function to search target in nums. If target exists, then return its index, otherwise return -1.


- Example 1:

  Input: nums = [-1,0,3,5,9,12], target = 9

  Output: 4

  Explanation: 9 exists in nums and its index is 4

- Example 2:

  Input: nums = [-1,0,3,5,9,12], target = 2

  Output: -1

  Explanation: 2 does not exist in nums so return -1
 

- Note:

  - You may assume that all elements in nums are unique.
  - n will be in the range [1, 10000].
  - The value of each element in nums will be in the range [-9999, 9999].

---

## 解题思路

这道题其实就是个二分查找。

- 自带Arrays.binarySearch()方法

  其实Java已经封装好二分查找的方法，只不过返回的不是-1，所以修改一下就好，时间复杂度是O(logn)

  ```
    public int search(int[] nums, int target) {
        int index = Arrays.binarySearch(nums, target);
		return index<-1?-1:index;
    }
  ```

- 自己实现

  自己实现一个二分查找也很简单，最开始检查整个数组，然后根据中值大小判断继续检查右边还是左边，循环到最后全部查完就可以。

  ```
    public int search(int[] nums, int target) {

        // 起始左右两边的索引位置
        int start = 0;
        int end = nums.length - 1;

        // 左边索引小于右边表示还没有全部遍历
        while (start <= end) {
            // 查询中值
            int middle = (start + end) / 2;
            long middleVal = nums[middle];

            // 中值比目标值小，查询中值右边
            if (middleVal < target) {
                start = middle + 1;
            }
            // 中值比目标值大，查询中值左边
            else if (middleVal > target) {
                end = middle - 1;
            }
            // 找到了目标值
            else {
                return middle;
            }
        }
        return -1;
    }

  ```