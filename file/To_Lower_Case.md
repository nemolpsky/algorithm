## To Lower Case
Implement function ToLowerCase() that has a string parameter str, and returns the same string in lowercase.

- Example 1:

  Input: "Hello"

  Output: "hello"

- Example 2:

  Input: "here"

  Output: "here"

- Example 3:

  Input: "LOVELY"

  Output: "lovely"

--- 

## 解题思路
这道题是要求将所有的大写字母都转换成小写字母

- ASCII码值与数字之间的转换

  字母A-Z是在65-90之间，a-z是在97-122，也就是说大写字母加上32就是对应的小写字母了，所以可以遍历字符串中的每个字符，如果大于等于65并且小于等于90的就是大写字母，需要加32来变成小写字母，也可以直接判断字符，Java底层会自动转换成数字判断。时间复杂度取决于字符串长度，也就是O(n)。

  ```
    public String toLowerCase(String str) {
        //判断控制
        if (str==null || str.length()==0){
            return null;
        }

        StringBuilder builder = new StringBuilder();
        for(int i=0;i<str.length();i++){
            char c = str.charAt(i);
            //if (c>='A' && c< 'a'){
            //大写字母需要转换
            if (c>=65 && c<= 90){
                builder.append((char)(c+32));
            }else {
                builder.append(c);
            }
        }
        return builder.toString();
    }
  ```
  
