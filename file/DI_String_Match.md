## DI String Match

Given a string S that only contains "I" (increase) or "D" (decrease), let N = S.length.

Return any permutation A of [0, 1, ..., N] such that for all i = 0, ..., N-1:

If S[i] == "I", then A[i] < A[i+1]
If S[i] == "D", then A[i] > A[i+1]
 

- Example 1:

  Input: "IDID"

  Output: [0,4,1,3,2]

- Example 2:

  Input: "III"

  Output: [0,1,2,3]

- Example 3:

  Input: "DDI"

  Output: [3,2,0,1]

---

## 解题思路
这道题目的题意理解起来确实有点困难，实际意思就是说会给定一个字符串，这个字符串里面只包含两种字符，分别是```D```和```I```，现在要求的是，根据规则返回一个数组，该数组是一个乱序的序列，范围就是0-N，N是数组长度。规则是假设数组长度为N，如果遇到```D```数字就先赋值再减1，数字从N开始减，如果遇到```I```数字就先赋值再加1，数字就0开始加。比如```DDI```，长度是3，第一个字符是```D```所以第一个数字就是3，第二字符也是```D```所以第二个数字是2，第三个字符是```I```所以第三个数字是0，最后一个数字就只能是1了。

- 遍历判断

  因为要判断是哪个字符，而且后面的值还是取决于前面的值，遍历是肯定跑不了了，设置好两种字符数字起始值，分别对应计算，要注意的是最后一个数字，原来不了解题意还专门去判断计算，最后发现并不需要，因为数组长度是N，数字的范围则是N+1，也就是0-N，也就是说遍历完后就只剩下一个数字了，这个时候IIndex和
  DIndex是一样的值，取任意其中一个值就可以了。

  ```
    int[] arr = new int[S.length() + 1];

    int IIndex = 0;
    int DIndex = S.length();

    for(int i=0;i<S.length();i++){

        char c = S.charAt(i);
        if (c == 'I'){
            arr[i] = IIndex++;
        }else{
            arr[i] = DIndex--;
        }
    }

    arr[arr.length-1] = IIndex;
    return arr;
  ```