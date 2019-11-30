## Lemonade Change

At a lemonade stand, each lemonade costs $5. 

Customers are standing in a queue to buy from you, and order one at a time (in the order specified by bills).

Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill.  You must provide the correct change to each customer, so that the net transaction is that the customer pays $5.

Note that you don't have any change in hand at first.

Return true if and only if you can provide every customer with correct change.

 

- Example 1:

  Input: [5,5,5,10,20]

  Output: true

  Explanation: 

  From the first 3 customers, we collect three $5 bills in order.
  From the fourth customer, we collect a $10 bill and give back a $5.
  From the fifth customer, we give a $10 bill and a $5 bill.
  Since all customers got correct change, we output true.

- Example 2:

  Input: [5,5,10]

  Output: true

- Example 3:

  Input: [10,10]

  Output: false

- Example 4:

  Input: [5,5,10,10,20]

  Output: false

  Explanation: 

  From the first two customers in order, we collect two $5 bills.
  For the next two customers in order, we collect a $10 bill and give back a $5 bill.
  For the last customer, we can't give change of $15 back because we only have two $10 bills.
  Since not every customer received correct change, the answer is false.
 

- Note:

  - 0 <= bills.length <= 10000
  - bills[i] will be either 5, 10, or 20.

---

## 解题思路

这道题目的意思是给定一个数组，表示每次顾客购买商品付的钞票的面额，加个是5美金，需要判断按数组的支付顺序是否能成功找零到最后一步。


- 统计钞票数量

  其实判断能不能找零就是要看小额钞票数量够不够，所以每一步都要统计当前5和10美金面额的钞票数量，要注意的就是对于20美金的找零是要优先取10+5的组合，其次再取3*5的组合，时间复杂度是O(n)。

  ```
	public boolean lemonadeChange(int[] bills) {

		// 存储5、15的钞票数量
		int fiveCount = 0;
		int tenCount = 0;

		for (int i : bills) {

			// 10块钱要找5块
			if (i == 10) {
				// 判断有没有5块
				if (fiveCount < 1) {
					return false;
				}
				// 减去一张5块
				fiveCount--;
			} 
			// 20块要找15，可以是3张5块或1张10块一张5块
			// 优先减去10+5，因为10+5只能用于20的找零，3*5对10和20的找零都可以用
			else if (i == 20) {

				// 判断10+5和3*5的组合是否存在
				if ((fiveCount >= 1 && tenCount >= 1) || fiveCount >= 3) {
					// 优先10+5
					if (fiveCount >= 1 && tenCount >= 1) {
						fiveCount--;
						tenCount--;
					} 
					// 最后才尝试3*5
					else {
						fiveCount -= 3;
					}
				} 
				// 两个组合都没有
				else {
					return false;
				}

			}

			// 记录5和10钞票的数量
			if (i == 5) {
				fiveCount++;
			} else if (i == 10) {
				tenCount++;
			}

		}

		return true;
	}
  ```