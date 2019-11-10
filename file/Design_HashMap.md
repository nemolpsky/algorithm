## Design HashMap

Design a HashMap without using any built-in hash table libraries.

To be specific, your design should include these functions:

put(key, value) : Insert a (key, value) pair into the HashMap. If the value already exists in the HashMap, update the value.
get(key): Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
remove(key) : Remove the mapping for the value key if this map contains the mapping for the key.

- Example:

  ```
  MyHashMap hashMap = new MyHashMap();
  hashMap.put(1, 1);          
  hashMap.put(2, 2);         
  hashMap.get(1);            // returns 1
  hashMap.get(3);            // returns -1 (not found)
  hashMap.put(2, 1);          // update the existing value
  hashMap.get(2);            // returns 1 
  hashMap.remove(2);          // remove the mapping for 2
  hashMap.get(2);            // returns -1 (not found) 
  ```

- Note:

  - All keys and values will be in the range of [0, 1000000].
  - The number of operations will be in the range of [1, 10000].
  - Please do not use the built-in HashMap library.
---

## 解题思路

这道题的意思是要自己设计一个HashMap，满足最基本的put、get和remove方法，不使用系统内置的HashMap。

- 数组

  HashMap的实现是散列函数，get方法是O(1)，这是因为一个key只能对应一个value，这是必须条件，而且存储的是key-value都是数字，key也全都是整数，所以可以直接使用数组，key和value的范围都是[0, 1000000]，所以只需要设置一个长度1000001的数组就可以。key作为索引，value就是值。

  ```
  class MyHashMap {

	int[] arr = new int[1000001];

	/** Initialize your data structure here. */
	public MyHashMap() {
		Arrays.fill(arr, -1);
	}

	/** value will always be non-negative. */
	public void put(int key, int value) {
		arr[key] = value;
	}

	/**
	 * Returns the value to which the specified key is mapped, or -1 if this map
	 * contains no mapping for the key
	 */
	public int get(int key) {
		return arr[key];
	}

	/**
	 * Removes the mapping of the specified value key if this map contains a mapping
	 * for the key
	 */
	public void remove(int key) {
		arr[key] = -1;
	}
  }
  ```