## Design HashSet

Design a HashSet without using any built-in hash table libraries.

To be specific, your design should include these functions:

add(value): Insert a value into the HashSet. 
contains(value) : Return whether the value exists in the HashSet or not.
remove(value): Remove a value in the HashSet. If the value does not exist in the HashSet, do nothing.

- Example:
  
  ```
  MyHashSet hashSet = new MyHashSet();
  hashSet.add(1);         
  hashSet.add(2);         
  hashSet.contains(1);    // returns true
  hashSet.contains(3);    // returns false (not found)
  hashSet.add(2);          
  hashSet.contains(2);    // returns true
  hashSet.remove(2);          
  hashSet.contains(2);    // returns false (already removed)
  ```
- Note:

  - All values will be in the range of [0, 1000000].
  - The number of operations will be in the range of [1, 10000].
  - Please do not use the built-in HashSet library.
---

## 解题思路
这道题是设计一个HashSet。

- 数组

  和前面设计HashMap非常像，只改动非常少的细节。

  ```
  class MyHashSet {

    int[] arr = null;
    /** Initialize your data structure here. */
    public MyHashSet() {
        arr = new int[1000001];
        Arrays.fill(arr,-1);
    }

    public void add(int key) {
        arr[key]=key;
    }

    public void remove(int key) {
        arr[key]=-1;
    }

    /** Returns true if this set contains the specified element */
    public boolean contains(int key) {
        return arr[key]!=-1;
    }
  }
  ```