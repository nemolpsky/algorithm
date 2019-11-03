## Last Stone Weight

We have a collection of rocks, each rock has a positive integer weight.

Each turn, we choose the two heaviest rocks and smash them together.  Suppose the stones have weights x and y with x <= y.  The result of this smash is:

If x == y, both stones are totally destroyed;
If x != y, the stone of weight x is totally destroyed, and the stone of weight y has new weight y-x.
At the end, there is at most 1 stone left.  Return the weight of this stone (or 0 if there are no stones left.)

 

- Example 1:

  Input: [2,7,4,1,8,1]

  Output: 1

  Explanation: 

  We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
  we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
  we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
  we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of last stone.
 

- Note:

  - 1 <= stones.length <= 30
  - 1 <= stones[i] <= 1000

---

## 解题思路

这道题非常有意思，给定一个数组，选取两个最大的数字，求差值，如果差值不等于0就把差值存入数组中，继续上述步骤，知道剩下最后一个数值，或者为空直接返回0。

- 有序队列

  这道题的思路非常清晰，难点在于怎么实现这个思路，相当于每次都可能插入一个随机数值到拿掉了两个最大值的数组中，无法保证排序，下一次获取两个最大值的时候就不好拿。所以使用PriorityQueue队列，这是一个有优先级的无界队列，可以调整下顺序，每次队列头部都弹出两个最大值，然后把差值存入，下次队列弹出的两个也是最大值，这就解决了排序问题。

  ```
	public static int lastStoneWeight(int[] stones) {
		// 队列每次从头部弹出最大值
		PriorityQueue<Integer> queue = new PriorityQueue<>(new Comparator<Integer>() {
			@Override
			public int compare(Integer o1, Integer o2) {
				return o2 - o1;
			}
		});

		// 数组全部存入队列
		for (int i : stones) {
			queue.add(i);
		}

		// 只剩一个数字前都要做互减操作
		while (queue.size()>1) {
			// 最大的两个值的差，不等于0就存入队列
			Integer firstMax = queue.poll();
			Integer secondMax = queue.poll();
			Integer value = Math.abs(firstMax - secondMax);
			if (value != 0) {
				queue.add(value);
			}
		}
		return queue.poll();
	}
  ```