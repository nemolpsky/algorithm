## BitSet原理和应用

BitSet是Java中封装好的一个提供对于位图(BitMap)进行操作的类。


---

### BitMap的含义

BitMap实际上就是计算机存储的单位bit，也就是二进制中的一个位数，每个bit可以只会有两个值，0或1，是计算机存储中的最小单位。其中8bit等于1Byte，1024Byte等于1KB，以此类推。而在Java中，比如一个32位的int整型数据就是说32bit大小，也就是4Byte大。

---

### BitSet的原理

- 构造方法

   BitSet初始化的时候会初始一个long类型的数组，源码中是进行了各种位运算比较难以理解，其实它一开始如果没指定构造方法的话会初始化长度为1的long类型数组，而实际的数据是存储在long数组中的long类型数据里面，一个long类型是64位，也就是说可以放64个bit数。

   ```
    private final static int ADDRESS_BITS_PER_WORD = 6;
    private final static int BITS_PER_WORD = 1 << ADDRESS_BITS_PER_WORD;

    private long[] words;

    public BitSet() {
        initWords(BITS_PER_WORD);
        sizeIsSticky = false;
    }

    private void initWords(int nbits) {
        words = new long[wordIndex(nbits-1) + 1];
    }

    private static int wordIndex(int bitIndex) {
        return bitIndex >> ADDRESS_BITS_PER_WORD;
    }
   ```
    
- set()/get()方法
    
   粗略的看下set()方法，就是利用或(|)操作来进行bit数据的存储，而expandTo()方法是一个判断是否要扩展的方法，所以每次BitSet扩展都是会增加64bit，也就是增加一个long类型，知道可以存储为止。

   ```
    public void set(int bitIndex) {
        if (bitIndex < 0)
            throw new IndexOutOfBoundsException("bitIndex < 0: " + bitIndex);

        int wordIndex = wordIndex(bitIndex);
        expandTo(wordIndex);

        words[wordIndex] |= (1L << bitIndex); // Restores invariants

        checkInvariants();
    }
   ```
   get()方法的本质则是利用与(&)操作来进行数据的操作。

   ```
    public boolean get(int bitIndex) {
        if (bitIndex < 0)
            throw new IndexOutOfBoundsException("bitIndex < 0: " + bitIndex);

        checkInvariants();

        int wordIndex = wordIndex(bitIndex);
        return (wordIndex < wordsInUse)
            && ((words[wordIndex] & (1L << bitIndex)) != 0);
    }
   ```

---

### 实际应用原理

可以看出BitSet最大的有点就是节省空间，1个int整型可以存32个bit数据，空间效率提升的非常高，那么实际的应用原理在哪里呢？举个例子，给定一个数组，范围是[0,100万]，里面有99万个无序不重复的数字，现在要求找出没有出现的那1万个数字，这个问题实现起来并不难，基本上都是遍历数组，然后将没出现数字的标记存储到List中，但是问题就是占用的容量太大了假如是使用一个List，里面存储99万个数字，那就是99万 * 4byte / 1024Byte / 1024KB ≈ 3.78MB，而如果换成bit则是99万 / 8bit / 1024Byte ≈ 120KB，要知道这种操作一般都是在内存中，而不是硬盘中，所以这个空间性能提升是非常重要的。

- 查找

  其实可以把BitSet看成一个大大的HashMap，但是它的占用空间非常小，当然这也是有代价的，每个bit只能保存1和0，所以也就只能判断数据是否存在之类的单一判断问题，如果想要判断次数单个BitSet肯定实现不了，所以BitSet非常适合海量大数据的操作，它还提供了很齐全的求交集，并集之类的封装方法。

  出名的布隆过滤器也是类似的原理，它根据散列函数计算出每条数据存储的位置，然后存储结构就是类似于BitSet一样的bit结构，所以它的局限性就是可能会有数据误判，因为散列函数是有可能计算出相同的哈希值的，这就表示一个对象可能并不应该拦截，但是它计算的哈希值和一个会被拦截的数据一样，那它就会被误判而拦截。

- 排序
  
  对于海量的不重复数字排序也可以使用BitSet，比如可以分批读取数字，然后存储到BitSet中，最后全部读取完之后可以遍历BitSet，标记true的是有值的直接输出，因为BitSet的索引是有序的，所以输出的就是排序后的结果。




