## Degree of an Array

Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.

- Example 1:

  Input: [1, 2, 2, 3, 1]

  Output: 2

  Explanation: 

  The input array has a degree of 2 because both elements 1 and 2 appear twice.
  
  Of the subarrays that have the same degree:

  [1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]

  The shortest length is 2. So return 2.

- Example 2:

  Input: [1,2,2,3,1,4,2]

  Output: 6

- Note:

  - nums.length will be between 1 and 50,000.
  - nums[i] will be an integer between 0 and 49,999.
---

## 解题思路

这道题是说给定一个非空正数数组，有重复的元素，重复最多的元素的重复次数是这个数组的度，需要做的是在这个数组中找到一个子数组，要求子数组的度也是和原来的数组一样的度，需要返回最短子数组的长度。

- 数组保存位置

  首先核心的思想就是要怎么找到最短的子数组，其实最短的子数组就是原来数组中出现最多次的那个元素第一次出现和最后一次出现的这一段，因为度是一样的，再短度就不一样了，再加上数组里面可能有多个数字是同样的长度，而位置又不同，也就是子数组长度不同，只需要找到子数组最短的那个元素即可，所以可以用map或者数组来保存元素个数找到度，再保存每个符合度的数组的开始和结束索引，然后求出差值最小的就是最短子数组的长度，时间复杂度是O(n)。


  ```
    public int findShortestSubArray(int[] nums) {
        // 存储每个元素的出现个数
        int[] count = new int[50000];
        int[] start = new int[50000];
        int[] end = new int[50000];
        // 开始数组填-1，防止刚好度的那个元素起始索引是0
        Arrays.fill(start,-1);
        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];
            // 累加度
            count[num]++;
            // 记录每个元素最开始索引
            if (start[num] == -1) {
                start[num] = i;
            }
            // 记录结束索引
            end[num] = i;
        }

        // 找出度
        int degree = 0;
        for (int i = 0; i < count.length; i++) {
            degree = Math.max(degree, count[i]);
        }


        // 符合度的元素，遍历哪个子数组最短
        int length = 50000;
        for (int i = 0; i < count.length; i++) {
            // 符合数组度的元素
            if (count[i] == degree) {
                length = Math.min(end[i] - start[i] + 1,length);
            }
        }

        return length;
    }
  ```