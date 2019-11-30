## Best Time to Buy and Sell Stock II

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

- Example 1:

  Input: [7,1,5,3,6,4]

  Output: 7

  Explanation: 
  
  Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.

  Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

- Example 2:

  Input: [1,2,3,4,5]

  Output: 4

  Explanation: 
  
  Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             
  Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.

- Example 3:

  Input: [7,6,4,3,1]

  Output: 0

  Explanation: 
  
  In this case, no transaction is done, i.e. max profit = 0.

---

## 解题思路

这道题是说给定一个数组，里面有乱序排列的值代表每天股票的价格，现在是要计算出买入再卖出的利润最大化值。

- 折线图分析

  这里是参考了LeetCode的答案，将数组中的数据画成一个折线图来分析，首先要知道什么时候才能利润最大化，肯定是在低价的时候买入，再等最高价的时候卖出，可以发现其实就是所有上升趋势两点之间的差。

  ![折线图](https://github.com/nemolpsky/algorithm/raw/master/file/image/maxprofit1.png)


  再进一步思考下，从每个x和y点去看，发现每个连续的上升趋势其实是整段上升趋势中单独的子段组成的，所以只要把所有比前一天价格高的进行差值计算相加就是总的差值。

  ![折线图](https://github.com/nemolpsky/algorithm/raw/master/file/image/maxprofit2.png)

  遍历元素，只要比前一个值大就计算差值并累加，时间复杂度是O(n)。

  ```
	public static int maxProfit(int[] prices) {
		int length = prices.length;
		int sum = 0;

		// 遍历
		for (int i = 0; i < length; i++) {
			// 如果后一个元素比前一个元素大，也就是上升趋势就加上这个差值
			if (i < length - 1 && prices[i] < prices[i + 1]) {
				sum += prices[i + 1] - prices[i];
			}
		}
		return sum;
	}
  ```