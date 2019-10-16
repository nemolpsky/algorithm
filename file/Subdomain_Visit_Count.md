## Subdomain Visit Count

A website domain like "discuss.leetcode.com" consists of various subdomains. At the top level, we have "com", at the next level, we have "leetcode.com", and at the lowest level, "discuss.leetcode.com". When we visit a domain like "discuss.leetcode.com", we will also visit the parent domains "leetcode.com" and "com" implicitly.

Now, call a "count-paired domain" to be a count (representing the number of visits this domain received), followed by a space, followed by the address. An example of a count-paired domain might be "9001 discuss.leetcode.com".

We are given a list cpdomains of count-paired domains. We would like a list of count-paired domains, (in the same format as the input, and in any order), that explicitly counts the number of visits to each subdomain.

- Example 1:

  Input: 

  ["9001 discuss.leetcode.com"]

  Output: 

  ["9001 discuss.leetcode.com", "9001 leetcode.com", "9001 com"]

  Explanation: 

  We only have one website domain: "discuss.leetcode.com". As discussed above, the subdomain "leetcode.com" and "com" will also be visited. So they will all be visited 9001 times.

- Example 2:

  Input: 

  ["900 google.mail.com", "50 yahoo.com", "1 intel.mail.com", "5 wiki.org"]

  Output: 

  ["901 mail.com","50 yahoo.com","900 google.mail.com","5 wiki.org","5 org","1 intel.mail.com","951 com"]

  Explanation: 

  We will visit "google.mail.com" 900 times, "yahoo.com" 50 times, "intel.mail.com" once and "wiki.org" 5 times. For the subdomains, we will visit "mail.com" 900 + 1 = 901 times, "com" 900 + 50 + 1 = 951 times, and "org" 5 times.

- Notes:

  - The length of cpdomains will not exceed 100. 
  - The length of each domain name will not exceed 100.
  - ach address will have either 1 or 2 "." characters.
  - The input count in any count-paired domain will not exceed 10000.
  - The answer output can be returned in any order.

--- 

## 解题思路
这道题的意思和前面根据邮箱地址判断多少个邮箱能收到邮件很像，首先会接受一个数组，每个字符串都包含一个数字和域名地址，按题目的说法数字代表了请求的次数，整个域名地址都被```.```分割成了子域名，比如请求```google.mail.com```就相当于分别请求了```google.mail.com```、```google.mail```和```com```三个域名各一次，所以要求计算出整个数组中出现的所有单独域名的请求次数，按格式返回。

- 遍历加HashMap

  一般像这种需要把整个字符串拆成不同的子串，然后又需要统计数量的很适合用HashMap，因为存取的时间复杂度都是O(1)，又可以使用key来保证唯一性。所以可以下面就是遍历数组，然后先去掉字符串中的数字部分，再按```.```分割成子域名，然后循环拼接，放入map中，如果map中已经存在这个key就累加更新次数。

  ```
    public static List<String> subdomainVisits(String[] cpdomains) {
        HashMap<String, Integer> map = new HashMap<>();

        List<String> list = new ArrayList<>();

        for (String cpdomain : cpdomains) {
            int index = cpdomain.indexOf(' ');
            // 请求次数
            int count = Integer.valueOf(cpdomain.substring(0, index));

            // 分割子域名
            String[] domains = cpdomain.substring(index + 1, cpdomain.length()).split("\\.");

            // 遍历子域名进行拼接
            for (int i =0;i<= domains.length - 1; i++) {
                // 从哪个索引开始拼接
                int l = i;
                int first = l;
                StringBuilder builder = new StringBuilder();
                while (l<=domains.length-1) {
                    // 第一次拼接的时候前面不加点
                    if (l!=first) {
                        builder.append(".");
                    }
                    builder.append(domains[l]);
                    l++;
                }
                // 存入map，如果key已经存在则直接更新value
                String key = builder.toString();
                int c = map.getOrDefault(key, 0);
                map.put(key, count + c);
            }
        }

        // 遍历map按格式返回数据
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            list.add(entry.getValue() +" " + entry.getKey());
        }

        return list;
    }
  ```