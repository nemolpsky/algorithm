## 二分查找

二分查找，顾名思义就是每次都用一个中点把数据集从中分成两半，然后判断这个中点的值比目标值大还是小，所以前提就是这个数据集是有序的，如果是升序的话，目标值比中点值大就要去查后半部分，小的话就要去查前半部分。还是去查那半部分的中点值，一直循环到最后，剩下的那个值就一定是目标值，这就是二分查找。

- 代码实现

  第一步就是要找到中点，然后每次查找完之后就再重新计算下中点，因此每次都必须重新计算头部索引和尾部索引。

  ```
    Integer[] arr = new Integer[]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20};
    List<Integer> nums = Arrays.asList(arr);

    private static int binarySearch(List<Integer> nums, int target) {
        //获取中间数的索引，第一次就是首尾除二
        int firstIndex = 0;
        int lastIndex = nums.size() - 1;
        int middleValue = 0;
        int middleIndex = nums.size()/2;

        int count = 1;
        // 首尾索引不相同
        while (firstIndex != lastIndex) {
            count++;
            // 取出中间的值
            middleValue = nums.get(middleIndex);
            System.out.println("index-" + middleIndex + ":value-" + middleValue + ":first-" + firstIndex + ":last-" + lastIndex);
            // 比目标大，在半部分的基础上找中点，尾部索引不变，头部索引是中间索引+1
            if (target > middleValue) {
                firstIndex = middleIndex + 1;
            }
            // 比目标大，在半部分的基础上找中点，头部索引不变，尾部索引是中间索引-1
            else if (target < middleValue) {
                lastIndex = middleIndex - 1;
            }
            // 值和目标值相同，找到了
            else {
                System.out.println(count);
                return middleIndex ;
            }
            // 每次都计算新的中间值
            middleIndex = (lastIndex+firstIndex)/2;
        }
        System.out.println(count);
        // 因为偶数的原因，最后可能会头尾索引是一样的，这个一样的索引就是目标值
        return firstIndex;
    }  
  ```

- 时间复杂度

  因为每次都是取中值，也就是说每次都是除以2，直到求出最后一个数，也就是说假设数据集长度是L，就要算出L除以2多少次。也就是说2的几次方大于等于L，用数学公式表达就是2ⁿ=L，n值就是要查找的次数，所n=log₂L，时间复杂度就是O(log₂L)。