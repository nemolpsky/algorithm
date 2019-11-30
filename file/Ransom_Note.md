## Ransom Note

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

- Note:
  
  You may assume that both strings contain only lowercase letters.

  ```
  canConstruct("a", "b") -> false
  canConstruct("aa", "ab") -> false
  canConstruct("aa", "aab") -> true
  ```

---

## 解题思路

这道题是说给定两个字符串，看第二个字符串中的字符是否可以组成第一个字符串，每个字符不能重复使用

- 数组记录字符

  统计两个字符串的字符，然后对比数量，是否能满足组成字符串的要求，时间复杂度是O(n)。

  ```
	public boolean canConstruct(String ransomNote, String magazine) {
		int length1 = ransomNote.length();
		int length2 = magazine.length();
		
		// 杂志字符串长度比信件字符串小，一定不满足条件
        if(ransomNote.length() > magazine.length()) {
            return false;
        }

		int[] arr = new int[26];

		// 两个字符串同时遍历
		for (int i = 0; i < Math.max(length1, length2); i++) {
			if (i < length1) {
				arr[ransomNote.charAt(i) - 'a']--;
			}

			if (i < length2) {
				arr[magazine.charAt(i) - 'a']++;
			}
		}

		// 遍历存储字符的数字是否有负数，负数表示杂志字符串不能满足信件的需求
		for (int i : arr) {
			if (i < 0) {
				return false;
			}
		}

		return true;
	}
  ```

- 数组记录字符优化

  ```
    public boolean canConstruct(String ransomNote, String magazine) {
		// 杂志字符串长度比信件字符串小，一定不满足条件
        if(ransomNote.length() > magazine.length()) {
            return false;
        }
        
        // 存储字符的次数
        int[] arr = new int[26];
        for(char c : ransomNote.toCharArray()) {
        	int start = arr[c - 'a'];
            int index = magazine.indexOf(c, start);
            if(index == -1) {
                return false;
            }
            arr[c - 'a'] = index + 1;
        }
        return true;
    }
  ```