## Unique Morse Code Words
International Morse Code defines a standard encoding where each letter is mapped to a series of dots and dashes, as follows: "a" maps to ".-", "b" maps to "-...", "c" maps to "-.-.", and so on.

For convenience, the full table for the 26 letters of the English alphabet is given below:

[".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."]

Now, given a list of words, each word can be written as a concatenation of the Morse code of each letter. For example, "cba" can be written as "-.-..--...", (which is the concatenation "-.-." + "-..." + ".-"). We'll call such a concatenation, the transformation of a word.

Return the number of different transformations among all words we have.

- Example:

  Input: words = ["gin", "zen", "gig", "msg"]

  Output: 2

  Explanation: 
  
  The transformation of each word is:
  - "gin" -> "--...-."
  - "zen" -> "--...-."
  - "gig" -> "--...--."
  - "msg" -> "--...--."

  There are 2 different transformations, "--...-." and "--...--.".

- Note:

  - The length of words will be at most 100.
  - Each words[i] will have length in range [1, 12].
  - words[i] will only consist of lowercase letters.

---

## 解题思路
本题的意思是说摩斯电码可以来表示不同的字母，现在给出了小写字母代表的摩斯电码，有时候不同的字母的摩斯电码连在一起可能刚好会组成一样的字符，比如```gin[--.|..|-.]```和zen```[--..|.|-.]```，连在一起刚好都是```--...-.```，所以题目是要求接受一个字符串数组，然后算出每个字符串的摩斯电码，然后返回去除掉重复之后的数量。

- 利用Set集合的唯一性

  直接遍历转换所有字符串的摩斯电码串，然后放入Set中，自动去除，最后Set长度就是不重复的摩斯电码串的数量

  ```
  public int uniqueMorseRepresentations(String[] words) {
    // a-z代表的摩斯电码
    String[] morseArr = new String[]{".-",
      "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--",         "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."};

    // 存放每个字符串的摩斯电码，可以去重
    Set<String> set = new HashSet<String>();
        
    // 遍历数组
    for (int i = 0; i < words.length; i++) {
       StringBuilder builder = new StringBuilder();
       // 遍历每个字符串的字符
       for (char c:words[i].toCharArray()) {
           // 因为题目提示中说了字符串一定是小写字母，也就是说当前字符的ASCII值减去'a'的值就可以得到数组中对应的莫斯电码，比如b是98，a是97，98-97为1，而索引为1的则是b对应的摩斯电码
           builder.append(morseArr[c - 'a']);
       }
       // 存入Set集合
       set.add(builder.toString());
    }
      
    return set.size();
  }
  ```