## Defanging an IP Address
Given a valid (IPv4) IP address, return a defanged version of that IP address.
A defanged IP address replaces every period "." with "[.]".

 

- Example 1:

  Input: address = "1.1.1.1"
  
  Output: "1[.]1[.]1[.]1"
  
- Example 2:
  
  Input: address = "255.100.50.0"
  
  Output: "255[.]100[.]50[.]0"
 

- Constraints:
  
  The given address is a valid IPv4 address.

---

## 解题思路

这道题目的意思是接受一个IPv4格式的字符串，需要把字符串中所有的```.```都替换成```[.]```

- replace()
   
  一般最省事的方法肯定是直接使用String的replace()方法，但是效率肯定不高，因为底层是正则匹配来实现的

- StringBuilder配合charAt()

  因为String的底层其实是一个char[]数组，而charAt()方法则是根据索引获取数组中的元素，时间复杂度是O(1)，因此整个方法的时间复杂度取决于address的长度，如果长度为n，则时间复杂度是O(n)，因为要循环n次来判断每个字符。

  ```
    public static String defangIPaddr(String address) {
        // 为空直接返回null
        if (address==null || address.length()==0){
            return null;
        }

        String str = "[.]";
        StringBuilder builder = new StringBuilder();

        // for循环遍历字符串
        for (int i=0;i<address.length();i++){
            // 判断每个字符是否是.
            char c = address.charAt(i);
            // 如果是则追加需要替换的字符串，否则还是追加原来的字符
            if (c=='.'){
                builder.append(str);
            }else {
                builder.append(c);
            }
        }
        return builder.toString();
    }
  ```