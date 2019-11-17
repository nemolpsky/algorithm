## Two City Scheduling

There are 2N people a company is planning to interview. The cost of flying the i-th person to city A is costs[i][0], and the cost of flying the i-th person to city B is costs[i][1].

Return the minimum cost to fly every person to a city such that exactly N people arrive in each city.

- Example 1:

  Input: [[10,20],[30,200],[400,50],[30,20]]

  Output: 110

  Explanation: 

  The first person goes to city A for a cost of 10.
  The second person goes to city A for a cost of 30.
  The third person goes to city B for a cost of 50.
  The fourth person goes to city B for a cost of 20.

  The total minimum cost is 10 + 30 + 50 + 20 = 110 to have half the people interviewing in each city.
 

- Note:

  - 1 <= costs.length <= 100
  - It is guaranteed that costs.length is even.
  - 1 <= costs[i][0], costs[i][1] <= 1000

---

## 解题思路

给定一个二维数组，其中子数组的长度是2，如果飞往A地，费用是子数组的索引0上的值，B地是索引1上的值，计算出一半人飞往A地，一半人飞往B地的最低费用。

- 数学思路

  如果直接去算的话，根本无从下手，所有从另一个思路下手，全都飞往A地，然后再选取AB两地值之间差最大的那一半飞往B地，这就相当于选出了飞往B地最便宜的一半人，另一半而是飞往A地最便宜的人，其实有点贪婪算法的影子在里头，确保飞往B地那一半每个人都是最便宜的，那自然而然总数就是最便宜的，时间复杂度是O(nlogn)。
  
  ```
	public static int twoCitySchedCost(int[][] costs) {

		// 人数
		int length = costs.length;
		// 所有人都飞往A的费用之和
		int sum = 0;

		int index = 0;
		int[] num = new int[length];
		for (int[] arr : costs) {
			sum += arr[0];
			// 两地费用之差
			num[index] = arr[0] - arr[1];
			index++;
		}

		// 排序
		Arrays.sort(num);

		// 选出一半人飞往B
		// 因为保存的是A-B的差值，求和是需要最小值
		// 所以看选差值最大的那一半，那表示A比B的费用是高的那一半，把这一半最高的换成B，自然得出了飞往AB最小的总费用了。
		for (int i = length / 2; i < length; i++) {
			sum -= num[i];
		}

		return sum;
	}
  ```