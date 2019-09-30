## Self Dividing Numbers

A self-dividing number is a number that is divisible by every digit it contains.

For example, 128 is a self-dividing number because 128 % 1 == 0, 128 % 2 == 0, and 128 % 8 == 0.

Also, a self-dividing number is not allowed to contain the digit zero.

Given a lower and upper number bound, output a list of every possible self dividing number, including the bounds if possible.

- Example 1:
  
  Input: 
  
  left = 1, right = 22

  Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]

- Note:
  
  The boundaries of each input argument are 1 <= left <= right <= 10000.

---

## 解题思路

这道题的意思是给两个不同的正值，然后从这两个数字之间找出所有符合要求的数字，所要求的就是数字可以除尽组成自己的所有数字，比如21分别除以2和1，31分别除以3和1，其中如果数字包含0则直接排除，比如10，201这样的。

- 遍历所有数字中的所有组成

  这个是最简单的方法，就是循环该范围内所有的数字，然后再循环它们的组成数字是否符合要求。但是这样效率很低，因为频繁的类型转换，尤其是当数字是接近10000的时候会越来越慢。
  
  ```
  public static List<Integer> selfDividingNumbers(int left, int right) {

	  List<Integer> list = new ArrayList<Integer>();

	  if (right < left) {
		return null;
	  }

	  for (int i = left; i <= right; i++) {
		  if (i == 0) {
			  continue;
		  }

		  char[] arr = (i + "").toCharArray();
		  int size = 0;
		  for (char c : arr) {
			  int num = Integer.valueOf(c + "");
			  if (num != 0 && i % num == 0) {
				  size++;
			  }
		  }
		  if (size == arr.length) {
			  list.add(i);
		  }
	  }

	  return list;
  }
  ```

- 取余运算

  使用运算符来判断就会效率高很多，先对每个数字取余，再除以10，这样循环下去就可以判断一个数字上所有位数上的数字，然后每次都判断尾部的数字就可以，时间复杂度是O(n)。
  
  ```
  public static List<Integer> selfDividingNumbers(int left, int right) {

	List<Integer> list = new ArrayList<>();
	for (int i = left; i <= right; i++)
		if (isDivide(i)) {
			list.add(i);
		}
	return list;

  }

  static boolean isDivide(int x) {
	int temp = x;
	
	// 循环作除10操作，就可以获得每个位数上的数字
	// 例如115
	// 第一次进来 115%10是5 得到个位数上的数 除以10去掉个位数上的数就是11
	// 第二次进来 11%10是1 得到是位数上的数 除以10去掉十位数就是1
	// 第三次次进来 1%10是1 得到是百位数上的数 除以10去掉十位数就是0 三个数字都判断完了
	while (x != 0) {
		
		int rem = x % 10;
		// 如果余数为0表示数字为0 
		if (rem == 0 || temp % rem != 0) {
			return false;
		}
		
		// 去掉尾部的数字
		x /= 10;
	}
	return true;
  }
  ```