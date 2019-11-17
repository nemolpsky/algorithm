## Add Digits

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

- Example:

  Input: 38

  Output: 2 

  Explanation: 
  
  The process is like: 3 + 8 = 11, 1 + 1 = 2. 
  Since 2 has only one digit, return it.

- Follow up:

  - Could you do it without any loop/recursion in O(1) runtime?

---

## 解题思路

这道题就是一道数学题，求数根。

- 求数根公式

  因为要求不适用循环和递归，所以只能使用数根公式计算，时间复杂度是O(1)。

  ```
	public static int addDigits(int num) {

		// 12,345 = 1×(9,999 +1)+ 2×(999 +1) + 3×(99 +1)+ 4×(9 +1)+ 5
		// 12,345 =(1×9,999 + 2×999 + 3×99 + 4×9) + (1 + 2 + 3 + 4 + 5)
		// 12,345 =(1×9,999 + 2×999 + 3×99 + 4×9) + [15%9(6)]

		// 38 = 3×(9 + 1) + 5
		// 38 = (3*9) + (3 + 8)
		// 38 = (3*9) + [11%9(2)]
		
        // 最终的求数根公式
        // return (num - 1) % 9 + 1;
		
		// 个位数的数根就是自己
        if(num<10){
            return num;
        }
        
        // 能除尽9的数根则是9
        if(num%9==0){
            return 9;
        }
        
        // 其他的都是对9求余
        return num%9;
	}

  ```