## Delete Columns to Make Sorted

We are given an array A of N lowercase letter strings, all of the same length.

Now, we may choose any set of deletion indices, and for each string, we delete all the characters in those indices.

For example, if we have an array A = ["abcdef","uvwxyz"] and deletion indices {0, 2, 3}, then the final array after deletions is ["bef", "vyz"], and the remaining columns of A are ["b","v"], ["e","y"], and ["f","z"].  (Formally, the c-th column is [A[0][c], A[1][c], ..., A[A.length-1][c]].)

Suppose we chose a set of deletion indices D such that after deletions, each remaining column in A is in non-decreasing sorted order.

Return the minimum possible value of D.length.


- Example 1:

  Input: ["cba","daf","ghi"]
  
  Output: 1

  Explanation: 

  After choosing D = {1}, each column ["c","d","g"] and ["a","f","i"] are in non-decreasing sorted order.If we chose D = {}, then a column ["b","a","h"] would not be in non-decreasing sorted order.

- Example 2:

  Input: ["a","b"]

  Output: 0

  Explanation: D = {}

- Example 3:

  Input: ["zyx","wvu","tsr"]

  Output: 3

  Explanation: D = {0, 1, 2}
 
- Note:

  - 1 <= A.length <= 100
  - 1 <= A[i].length <= 1000

---

## 解题思路
这道题目的意思是真的比较难理解，意思就会给定一个数据，里面都是由小写字母组成的字符串，每个字符串的长度都一样，要比较每个字符串上相同索引的字符，只要是降序的就需要删除掉所有字符串下该索引的值，最后返回删除的次数。比如```["cba","daf","ghi"]```，索引0的对比就是```c->d->g```，索引1的对比就是```b->a->h```，索引2的对比就是```a->f->i```，可以发现0和2位置的顺序都是升序的，只有1位置不是，```b->a```是降序，所以该索引要删除掉，所以删除的个数就1个。

- 遍历对比

  本质其实就是把所有的字符串相同的索引进行对比，只要和下一个比是降序，该索引就要删除，也不用继续往下比了。时间复杂度是O(n)，n是数组长度。

  ```
    public int minDeletionSize(String[] A) {

        int count = 0;

        // 每个字符串的长度，都是一致的
        int strLength = A[0].length();

        // 遍历每个字符
        for(int i=0;i<strLength;i++){
            // 遍历数组
            for(int l=0;l<A.length;l++){
                // 对比每个字符串和下个字符串相同索引位置的ASCII值，前面的大于后面的就是降序，需要删除
                if (A[l].charAt(i)>A[l+1].charAt(i)){
                    count++;
                    break;
                }
            }
        }

        return count;
    }
  ```