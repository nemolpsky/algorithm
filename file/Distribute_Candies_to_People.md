## Distribute Candies to People

We distribute some number of candies, to a row of n = num_people people in the following way:

We then give 1 candy to the first person, 2 candies to the second person, and so on until we give n candies to the last person.

Then, we go back to the start of the row, giving n + 1 candies to the first person, n + 2 candies to the second person, and so on until we give 2 * n candies to the last person.

This process repeats (with us giving one more candy each time, and moving to the start of the row after we reach the end) until we run out of candies.  The last person will receive all of our remaining candies (not necessarily one more than the previous gift).

Return an array (of length num_people and sum candies) that represents the final distribution of candies.

- Example 1:

  Input: candies = 7, num_people = 4

  Output: [1,2,3,1]

  Explanation:
  
  On the first turn, ans[0] += 1, and the array is [1,0,0,0].
  On the second turn, ans[1] += 2, and the array is [1,2,0,0].
  On the third turn, ans[2] += 3, and the array is [1,2,3,0].
  On the fourth turn, ans[3] += 1 (because there is only one candy left), and the final array is [1,2,3,1].

- Example 2:

  Input: candies = 10, num_people = 3

  Output: [5,2,3]

  Explanation: 

  On the first turn, ans[0] += 1, and the array is [1,0,0].
  On the second turn, ans[1] += 2, and the array is [1,2,0].
  On the third turn, ans[2] += 3, and the array is [1,2,3].
  On the fourth turn, ans[0] += 4, and the final array is [5,2,3].

---

## 解题思路
这道题的意思是给定两个数字，分别是糖果数量和人数，需要分糖果，分的规则是从第一个人开始分1个，往后每次糖果数加1，如果分完一轮还没结束就再继续一轮，需要列出所有人分到的糖果数量。

- while循环

  其实这道题思路还是比较清晰的，遍历人数分糖果，每次数量加1， 如果糖果还没分完就回到第一个人再分配一轮，直到糖果分完为止。时间复杂度是O(n)，n²+n/2 < candies。

  ```
  //		60个糖果，4个人
  //		1 2 3 4递增数列顺序分，数列之和是(1+n)n/2 = 60
  // 		n(n+1)/2 < 60
  //		n(n+1) < 120
  //		n²+n < 120
  //		n²+n-120 < 0
  //        n < 12
  //        所以在60个糖果，4个人的情况下要循环11次。
		
		
  //		   +1   +2   +3   +4   50
  //            1    2    3    4
  //		   +5   +6   +7   +8   24
  //            6    8    10   12
  //		   +9   +10  +5   +0
  //            15   18   15   12
  ```

  ```
	public static int[] distributeCandies(int candies, int num_people) {

		int[] arr = new int[num_people];

		// 结束标记
		boolean isOver = false;

		while (!isOver) {
			// 初始分糖果数
			int candiesNum = 1;
			// 遍历分给所有人
			for (int i = 1; i <= num_people; i++) {
				if (candies < candiesNum) {
					candiesNum = candies;
					isOver = true;
				} else {
					// 当前剩余糖果数减去当前分的糖果数
					candies -= candiesNum;
				}
				arr[i - 1] = candiesNum + arr[i - 1];
				
				// 结束了就不再遍历了
				if (isOver) {
					break;
				}

				// 糖果数每次都加1
				candiesNum++;

				if (i == num_people && !isOver) {
					i = 0;
				}
			}
		}
		return arr;
	}
  ```