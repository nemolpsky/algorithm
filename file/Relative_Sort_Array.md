## Relative Sort Array

Given two arrays arr1 and arr2, the elements of arr2 are distinct, and all elements in arr2 are also in arr1.

Sort the elements of arr1 such that the relative ordering of items in arr1 are the same as in arr2.  Elements that don't appear in arr2 should be placed at the end of arr1 in ascending order.

 

- Example 1:

  Input: arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]

  Output: [2,2,2,1,4,3,3,9,6,7,19]
 
- Constraints:

  - arr1.length, arr2.length <= 1000
  - 0 <= arr1[i], arr2[i] <= 1000
  - Each arr2[i] is distinct.
  - Each arr2[i] is in arr1.

--- 

## 解题思路

这道题目的意思是给定两个数组，其中数组2的长度会比数组1小，数组1是数组2的子集，要求返回一个数组，包含数组1中所有的值，顺序则是要和数组2中的一致，而那些数组2中没有的值，则按升序放最后。

- LinkedHashMap
  
  难度在于是前半截要按数组2的顺序，后半截则是要升序，而且数组1中还会有重复数字，所以肯定是要使用map这种key-value形式来保存，这样才能记录次数，而LinkedHashMap则在这个基础上还可以保存元素的输入顺序，前半截的问题就解决了。而后半截是要按升序，所以直接把数组1排序，因为最后是遍历的map来存值，这样遍历数组2存值保留了顺序，再遍历数组1存值则保留了多余数字的升序排列。时间复杂度是O(nlogn)，主要是在排序上。

  ```
	public static int[] relativeSortArray(int[] arr1, int[] arr2) {
		// 数组1排序
		Arrays.sort(arr1);

		// 可以保证输入顺序
		LinkedHashMap<Integer, Integer> map = new LinkedHashMap<>();

		// 将数组2中的数字存入map中
		for (int i = 0; i < arr2.length; i++) {
			map.put(arr2[i], 0);
		}

		// 再将数组1中的数字次数也存入map中
		for (int i = 0; i < arr1.length; i++) {
			int key = arr1[i];
			int value = map.getOrDefault(arr1[i], 0);
			map.put(key, value + 1);
		}

		// 新建一个数组存储
		int[] newArr = new int[arr1.length];
		int arrIndex = 0;

		// 遍历map
		for (Entry<Integer, Integer> entry : map.entrySet()) {
			// 获取数字出现的次数
			int count = entry.getValue();
			// 获取key，也就是数字
			int value = entry.getKey();

			// 只要次数没有为0就一直按顺序存放到数组中
			while (count-- > 0) {
				newArr[arrIndex++] = value;
			}
		}

		return newArr;
	}
  ```

- 数组

  其实数组也可以作为一个天然有顺序的map来用，索引就是key，存储的则是value，全都存放到一个数组中后，再遍历数组2放置前半截元素，然后再遍历存放数据的数组，因为已经是升序的，直接放置就可以满足后半截数据的要求，时间复杂度是O(n)。

  ```
	public static int[] relativeSortArray(int[] arr1, int[] arr2) {
		// 数值分布是0-1000之间，所以长度是1001
		int[] sortedArr = new int[1001];

		// 将数组当做map使用，索引是key，次数是value，还是天然升序的
		for (int i = 0; i < arr1.length; i++) {
			sortedArr[arr1[i]]++;
		}

		// 返回的数组
		int[] newArr = new int[arr1.length];
		int arrIndex = 0;
		// 遍历数组2
		for (int i = 0; i < arr2.length; i++) {
			// 索引就是key，也就是数组1中出现的数字
			int key = arr2[i];
			// 次数就是value，也就是数组1中出现的数字的次数
			int count = sortedArr[key];
			// 按顺序放入数组中，直到次数自减为0
			while (count-- > 0) {
				newArr[arrIndex++] = key;
				// 减少数组中的该数字的次数
				sortedArr[key]--;
			}
		}

		// 上面的步骤将所有数组2中的数字放入了新数组中，现在还剩数组1中只有的数字没有放入
		// 遍历存放次数的数组
		for (int i = 0; i < sortedArr.length; i++) {
			// 因为上面的步骤会把数字的对应次数减到0，所以值不为0的就是还没有放入的
			// 而且因为是索引是升序的，数值大小也是排好序的，直接按遍历放入集合中就好了
			// 直接接着上面新数组遍历到的索引位置继续就可以
			int count = sortedArr[i];
			while (count > 0) {
				newArr[arrIndex++] = i;
				count--;
			}
		}
		
		return newArr;
	}
  ```
