## Unique Email Addresses

Every email consists of a local name and a domain name, separated by the @ sign.

For example, in alice@leetcode.com, alice is the local name, and leetcode.com is the domain name.

Besides lowercase letters, these emails may contain '.'s or '+'s.

If you add periods ('.') between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, "alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address.  (Note that this rule does not apply for domain names.)

If you add a plus ('+') in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered, for example m.y+name@email.com will be forwarded to my@email.com.  (Again, this rule does not apply for domain names.)

It is possible to use both of these rules at the same time.

Given a list of emails, we send one email to each address in the list.  How many different addresses actually receive mails? 

- Example 1:

  Input: ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]

  Output: 2

  Explanation: "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails
 

- Note:

  - 1 <= emails[i].length <= 100
  - 1 <= emails.length <= 100
  - Each emails[i] contains exactly one '@' character.
  - All local and domain names are non-empty.
  - Local names do not start with a '+' character.

---

## 解题思路

这道题目涉及到的就是关于字符串的匹配和替换，意思是会给定一个字符串数组，里面全都是邮箱地址格式，前半截名字中可能会含有```.```和```+```符号，对于```.```是直接去除，```+```符号则是去除```+```和后面所有的字符串，最后去重统计会有多少邮箱地址。

- 遍历判断+Set

  首先遍历整个数组时必须的，然后只需要判断```@```前的内容，先判断```+```号，去除掉后面的内容，再去除```.```，最后放入Set集合中，自动去重，时间复杂度是O(n)，判断和调用都是用的Java自带的封装方法。

  ```
    public int numUniqueEmails(String[] emails) {
		HashSet<String> set = new HashSet<>();

		for (String email : emails) {
			int index = email.indexOf("@");
			String prefix = email.substring(0, index);
			String suffix = email.substring(index, email.length());
            
            if (prefix.contains("+")) {
				int endIndex = prefix.indexOf("+");
				prefix = prefix.substring(0,endIndex);
			}
			
			if (prefix.contains(".")) {
				set.add(prefix.replace(".", "") + suffix);
			}else {
				set.add(prefix + suffix);
			}

		}
		return set.size();
    }
  ```