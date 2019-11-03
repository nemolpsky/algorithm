## Fizz Buzz

Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

- Example:

  n = 15,

  Return:
  ```
  [
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
  ]
  ```
---

## 解题思路

给定一个数字，从1开始到这个数字，同时整除3和5输出FizzBuzz，只能整除3输出Fizz，只能整除5输出Buzz，都不满足直接输出数字。

- 遍历取余判断

  遍历取余判断每个数字，分别输出不同的值。

  ```
    public static List<String> fizzBuzz(int n) {
        List<String> list = new ArrayList<>(n);
        for(int i=1;i<=n;i++){
            if (i%3==0 && i%5==0){
                list.add("FizzBuzz");
            }else if(i%3==0){
                list.add("Fizz");
            }else if(i%5==0){
                list.add("Buzz");
            }else{
                list.add(i+"");
            }
        }
        return list;
    }
  ```