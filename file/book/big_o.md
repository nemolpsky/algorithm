## 大O表示法

算法的目的是为了加快代码的运行效率，那肯定会有个计算标准来对比不同的算法之间的效率，一般是使用大O表示法。

- 格式

  一般固定包含大写的O，再加括号，括号里面则是代表运算次数的代数，例如O(n)就表示要运算n次，而n次则是代表着该算法最糟糕的情况下的运算次数。

- 计算方式

  在代码中一般执行一行代码就算一次，而大O表示法则是在计算出所有的执行次数的基础上只保留最高项，连最高项的系数也不保留。
  

  - 比如下列的代码执行了3次，时间复杂度就是O(3)
    ```
    public void test(){
      System.out.println(1);
      System.out.println(1);
      System.out.println(1);
    }
    ```

  - 下列代码则执行了2n次，时间复杂度就是O(n)

    ```
    public void test(int n){

      for(int i=0;i<2n;i++){
          System.out.println(1);
          System.out.println(1);
          System.out.println(1);
        }
    }
    ```
 
  - 下列代码则执行了n²+n次，时间复杂度就是O(n²)

    ```
    public void test(int n){

      for(int i=0;i<n;i++){
          for(int l=0;l<n;l++){
              System.out.println(1);
              System.out.println(1);
              System.out.println(1);
          }
      }

      for(int i=0;i<n;i++){
          System.out.println(1);
          System.out.println(1);
          System.out.println(1);
      }
    }
    ```

- 常见的时间复杂度表示
  - O(log₂n)，也叫对数时间，例如二分查找。
  - O(n)，也叫线性时间，例如简单查找。
  - O(n*log₂n)，例如快速排序。
  - O(n²)，例如选择排序。