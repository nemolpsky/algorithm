## Unique Number of Occurrences

Given an array of integers arr, write a function that returns true if and only if the number of occurrences of each value in the array is unique.

- Example 1:

  Input: arr = [1,2,2,1,1,3]

  Output: true

  Explanation: The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.

- Example 2:

  Input: arr = [1,2]

  Output: false

- Example 3:

  Input: arr = [-3,0,1,-3,1,1,1,-3,10,0]

  Output: true
 

- Constraints:

  - 1 <= arr.length <= 1000
  - -1000 <= arr[i] <= 1000

---

## 解题思路
这道题目的意思是给定一个数组，有重复的元素，需要判断所有元素的重复次数，如果刚好每个元素的重复次数都不一样就返回true，否则返回false。

- HashMap

  首先是利用map来保存每个元素的出现次数，再判断map里面的value有没有重复就可以，这里取巧放到了set中，借助去重的特性对比长度，时间复杂度是O(n)。
  
  ```
    public boolean uniqueOccurrences(int[] arr) {

        // 将所有数字出现的次数存入map
        HashMap<Integer, Integer> map = new HashMap<>(1000);
        for (int a : arr) {
            int value = map.getOrDefault(a, 0);
            map.put(a, value + 1);
        }
        
        // map的长度
        int size = map.size();
        
        // 将map的values放入set，因为自动去重的原因，如果有重复的话，map和set的大小会不一样
        HashSet<Integer> set = new HashSet<>(map.values());
        if (size == set.size()) {
            return true;
        } else {
            return false;
        }
    }
  ```

- 数组

  其实数组是一个天然的map，索引做key，数值做value，要注意的是元素的范围是-1000到1000，所以数组长度是2001，而且因为有负数的缘故所以每次存储的时候需要加1000，这样最小值-1000就存到了0的位置，1000则存到了2000的位置，然后还需要一个保存次数的数组，索引代表有多少个元素出现这么多次次数，最后再遍历这个数组即可，时间复杂度是O(n)。

  ```
    public static boolean uniqueOccurrences(int[] arr) {
        // 有N个元素，最极端的情况就是平均每个元素出现一次，长度N就足够了
        int[] countArr = new int[arr.length];
        // 数值范围-1000至1000，也就是最多需要2001个位置
        int[] newArr = new int[2001];
        for (int a : arr) {
            // -1000存到索引0，1000存到索引2000
            // 存储完后自增值，也就是出现的次数
            int count = newArr[a+1000]++;

            // 次数大于0，就需要删掉在上一个次数索引的数值
            if (count>0){
                countArr[count]--;
            }

            // 在自增后的次数索引出的数值进行自增
            countArr[count+1]++;
        }

        // 如果有大于1的就是表示该次数有超过一个元素是这个个数
        for(int a:countArr){
            if (a>1){
                return false;
            }
        }
        return true;
    }
  ```