### 倒排索引：

倒排索引经常用于搜索引擎的检索功能中，大到百度、谷歌搜索，小到APP或者是电商系统中的搜索，比如在QQ阅读中的书城里搜索书籍，又或者是在书中搜索某个词组，以及在淘宝等商城搜索各种上之类的，其底层的核心原理都是基于倒排索引，而现如今比较流行的Elasticsearch就是一个基于倒排索引原理的分布式搜索引擎。

---

### 原理介绍：

其实倒排索引是的英文名是Inverted index，翻译成倒排索引比较让人误解，有很多人喜欢称之为转置索引或者是反向索引。其实意思非常简单，比如下面这个例子，第一个正向索引的意思是text1的文本里面包含了我们两个字，而倒排索引就是基于正向索引反过来，我们这个词是存放在text1中。

仅仅是颠倒一下意义就不同了，假设有10万个类似text1这样的文本，而每个文本里面都有大量的不同词组，那如果依赖正向索引去查找一个特定的词组的话，我们是没法直接知道哪个文档里面包含这个词组，只能遍历所有的文档，然后遍历它们的索引来查找，在搜索引擎这种海量数据的场景下这种方式根本不可行。

但是倒排索引就可以解决这个问题，如果我们想查找某个特定的词组，直接去查找它对应的索引看看有哪些文档存在它就可以了，这样就解决了上面遍历的问题，当然响应的也会带来索引数据量很大的问题。

```
// 正向索引
text1  ->  我们
// 倒排索引
我们  ->  text1
```

---

### 实践过程：

在实际应用的过程中，搜索引擎其实会解析文本数据先建立一个正向索引，然后再根据这个正向索引建立一个倒排索引，这也就是为啥会被称之为倒排索引的原因，本质上它就是一个普通的索引罢了。

下面这段代码模拟了这个过程，当然真实情况下会比这个复杂的多，这就设计到大数据还有算法领域的技术了，比如数据的清洗，然后就是数据的分析，以及句子的分词之类的问题，但是这个代码可以更好的理解倒排索引的本质原理和概念。

```
public class Test {

    // 存储正向索引的map
    public static Map<String,String> map = new HashMap<>();
    // 存储倒排索引的map
    public static Map<String,String> invertedIndexMap = new HashMap<>();
    
    // 三个不同的句子，已经手动模拟使用空格进行了分词
    public static String text1 = "理解 正向 索引";
    public static String text2 = "正排 索引 和 倒排 索引";
    public static  String text3 = "什么 是 倒排 索引 与 正向 索引";

    public static void main(String[] args) {
        // 解析文本
        parse(text1,"text1");
        parse(text2,"text2");
        parse(text3,"text3");

        // 打印正向索引
        System.out.println(map);
        // 创建倒排索引
        createInvertedIndex();
        // 打印倒排索引
        System.out.println(invertedIndexMap);
    }

    public static void createInvertedIndex(){
        // 遍历正向索引的map
        for(Map.Entry<String,String> entry:map.entrySet()){
            // 获取value进行截取
            for(String str:entry.getValue().split(",")) {
                // 获取key
                String key = entry.getKey();
                // 使用value当做key来查询反向索引的map
                String oldStr = invertedIndexMap.get(str);
                if (oldStr != null) {
                    key = oldStr + "," + key;
                }
                invertedIndexMap.put(str, key);
            }
        }
    }

    public static void parse(String text,String textName){
        // 使用空格进行分词，然后将其存入map，建立对应的正向索引
        String[] words = text.split(" ");
        for (String word:words){
            String oldStr = map.get(textName);
            if (oldStr!=null){
                word = oldStr+","+word;
            }
            map.put(textName,word);
        }
    }
}

```

下面可以看到输出结果，倒排索引就是把正向索引给翻转了，当然因为只是简单的模拟这里没有对反向索引里的内容进行去重。

```
// 正向索引
{text3=什么,是,倒排,索引,与,正向,索引, text1=理解,正向,索引, text2=正排,索引,和,倒排,索引}
// 倒排索引
{什么=text3, 正排=text2, 正向=text3,text1, 理解=text1, 倒排=text3,text2, 索引=text3,text3,text1,text2,text2, 和=text2, 与=text3, 是=text3}
```

---

### 总结：
- 倒排索引本质上就是一个普通的索引，只不过是基于正向索引翻转生成的，所以被称为倒排索引。
- 倒排索引的使用会产生更多的索引数据，对空间使用量的需求会增大。
- 例如Elasticsearch等搜索引擎就是利用了倒排索引的原理，因为正向索引是无法实现如此大量数据的搜索。