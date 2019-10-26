##  Play with Chips

There are some chips, and the i-th chip is at position chips[i].

You can perform any of the two following types of moves any number of times (possibly zero) on any chip:

Move the i-th chip by 2 units to the left or to the right with a cost of 0.
Move the i-th chip by 1 unit to the left or to the right with a cost of 1.
There can be two or more chips at the same position initially.

Return the minimum cost needed to move all the chips to the same position (any position).

 

- Example 1:

  Input: chips = [1,2,3]

  Output: 1

  Explanation: Second chip will be moved to positon 3 with cost 1. First chip will be moved to position 3 with cost 0. Total cost is 1.

- Example 2:

  Input: chips = [2,2,2,3,3]

  Output: 2

  Explanation: Both fourth and fifth chip will be moved to position two with cost 1. Total minimum cost will be 2.
 

- Constraints:

  - 1 <= chips.length <= 100
  - 1 <= chips[i] <= 10^9
---

## 解题思路
这道题的意思是说有一堆筹码，放在不同的位置上，用一个数组表示各个筹码的位置，比如[2,2,2,3,3]就表示有5个筹码，前三个都是在位置2上，后两个在位置3上，要求是把所有的筹码放到同一个位置上，移动规则是移动两步+0，移动一步+1，最后计算总步数。

- 数学方式

  这种题目感觉是类似于一种需要数学思维的脑筋急转弯的题目，首先需要知道的如果移动一个元素，而无论移动多少步数，只需要看它移动步数是奇数还是偶数，偶数不用考虑，只要考虑奇数+1的情况，偶数位置移动到偶数位置，奇数移动到奇数位置都是+0，因为移动的步数都是偶数的，所以只要看是把奇数移动到偶数，还是把偶数移动到奇数，这两类操作都会+1，而我们需要最小值，所以选择移动最少的元素，所以只要看奇数元素和偶数元素哪个少，时间复杂度是O(n)。

  ```
    public static int minCostToMoveChips(int[] chips) {
        // 奇数偶数的数量
        int odd = 0;
        int even = 0;

        // 遍历统计奇数和偶数的数量
        for(int i:chips){
            if (i%2==0){
                even++;
            }else{
                odd++;
            }
        }

        // 想要获取最小的步数，移动最少的元素，所以看奇数偶数元素哪个最少
        return Math.min(odd,even);
    }
  ```