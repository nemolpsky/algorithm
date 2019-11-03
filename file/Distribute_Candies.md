## Distribute Candies

Given an integer array with even length, where different numbers in this array represent different kinds of candies. Each number means one candy of the corresponding kind. You need to distribute these candies equally in number to brother and sister. Return the maximum number of kinds of candies the sister could gain.
- Example 1:

  Input: candies = [1,1,2,2,3,3]

  Output: 3

  Explanation:

  There are three different kinds of candies (1, 2 and 3), and two candies for each kind.
  Optimal distribution: The sister has candies [1,2,3] and the brother has candies [1,2,3], too. 
  The sister has three different kinds of candies. 

- Example 2:
 
  Input: candies = [1,1,2,3]

  Output: 2

  Explanation: For example, the sister has candies [2,3] and the brother has candies [1,1]. 
  The sister has two different kinds of candies, the brother has only one kind of candies. 

- Note:

  - The length of the given array is in range [2, 10,000], and will be even.
  - The number in given array is in range [-100,000, 100,000].
---

## 解题思路

这道题感觉更像是一道数学方面的思维题，给定一个数组，数组长度是偶数，数字范围是[-100,000, 100,000]，元素可能会重复，不同的数字代表不同种类的糖果，现在分糖果给两个人，平均分，问一个人能分到的最多不同种类的糖果数是多少。

- Set去重

  首先最重要的就是知道有多少种类的糖果，set是最方便的，直接去重去长度即可。然后其实就两种情况，第一种就是糖果种类数量大于每个人均分的数量，也就是总数量的一半，这个时候最多能分到一半的种类不同的数量，因为是均分，没法超过一半，第二种就是种类不到一半，所以能分到的最多种类数量就是当前种类数量，剩下的不足的则由相同的种类补齐一半的数量，保证均分。

  ```
    public static int distributeCandies(int[] candies) {

        // 存放所有种类的糖果
        HashSet<Integer> set = new HashSet<Integer>();
        for (int i : candies) {
            set.add(i);
        }

        // 每人得到的糖果数
        int length = candies.length / 2;

        // 最多就是得到的糖果数都是不同的，也就是一半
        // 如果糖果种类数还不到一半，那最多的数量就是糖果种类的数量
        return Math.min(length,set.size());
    }
  ```