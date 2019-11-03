## Next Greater Element I

You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

- Example 1:

  Input: nums1 = [4,1,2], nums2 = [1,3,4,2].

  Output: [-1,3,-1]

  Explanation:
  ```
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
  ```

- Example 2:

  Input: nums1 = [2,4], nums2 = [1,2,3,4].

  Output: [3,-1]

  Explanation:
  ```  
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
  ```

- Note:
  - All elements in nums1 and nums2 are unique.
  - The length of both nums1 and nums2 would not exceed 1000.
---

## 解题思路

这道题的描述也是很有歧义，说的是给定两个数组，数组1是数组2的子集，需要找到数组1中所有的值在数组2中对应的位置，看在该数字的右边离它最近的比它大的是多少，如果没有则返回-1。

- HashMap加队列

  这道题的核心起就是要找出数组2中所有数字右边离它最近比他大的数字。重要的是要怎么找到比自己大离自己最近的数字，最笨的方法肯定就是直接遍历右边，效率肯定会很低，所以使用一个队列存储，遍历到下一个元素的循环看是否比队列中的元素大，只要比较队尾的即可，队列中积压的数字一定是降序的，否则就不可能积压了，到最后全都遍历完了还积压数字就是找不到最大值，统一填-1，时间复杂度是O(n)。

  ```
    public static int[] nextGreaterElement1(int[] nums1, int[] nums2) {
        //队列
        LinkedList<Integer> linkedList = new LinkedList<>();

        //存放每个值右边最近的大于自己的值
        HashMap<Integer, Integer> map = new HashMap<>();

        int[] arr = new int[nums1.length];
        if (nums1.length==0){
            return arr;
        }

        linkedList.add(nums2[0]);

        for (int i = 1; i < nums2.length; i++) {
            int value = nums2[i];
            // 看该索引上的值是不是有比队列中的某个值大
            while (!linkedList.isEmpty() && linkedList.peekLast()<nums2[i]){
                map.put(linkedList.pollLast(),value);
            }
            // 将当前值存入队列
            linkedList.add(value);
        }

        // 此时队列中剩余的数字都是没有找到最大值的
        while(!linkedList.isEmpty()){
            map.put(linkedList.pollLast(),-1);
        }

        // 遍历数组1寻找map中对应的值
        for(int i=0;i<nums1.length;i++){
            arr[i] = map.get(nums1[i]);
        }

        return arr;
    }
  ```

