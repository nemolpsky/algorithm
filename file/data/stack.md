## 堆栈

1. 数据结构

   堆栈，也被称为栈，底层的数据结构其实是一种特殊的线性表结构，线性表结构的特点就是元素之间是相互连接的，每个元素都有一个前置元素，除了头部元素之外，每个元素都有一个后置元素，除了尾部元素之外。只不过线性表是允许在任意位置进行元素的插入和删除，而栈则不行，栈遵循的原则是先进后出(FILO)，可以想象想忘一个抽屉里放文件，每次只放张，只能放在上次放的文件的上面，而取的时候也每次只能取一张，只能取最上面的，这就是栈。

   - 图片

2. 自己实现一个栈

   先是定义好节点的对象结构，有两个属性，分别是存储的值和前置节点。然后就是每次入栈的时候都要创建一个新节点来保存值，然后帮上次创建的节点作为当前节点的前置节点，出栈的时候则需要将当前节点弹出，当前节点的前置节点作为最顶层的节点。

   ```
   public class SimpleStack<T> {

    // 当前节点
    private Node currentNode;
    // 前置节点
    private Node pre;
    // 设置长度
    private int length;
    // 长度
    private int size;

    public SimpleStack() {
        this.length = -1;
    }

    // 可以限定栈的操作
    public SimpleStack(int length) {
        this.length = length;
    }

    // 入栈操作
    public void push(T value){
        if (length>0 && size>= length){
            return ;
        }

        // 累计长度
        count(true);

        // 第一次添加的时候
        if (currentNode==null) {
            // 创建当前节点，填入数值
            currentNode = new Node();
            currentNode.value = value;
        }
        // 第一次之后的操作
        else{
            // 当前节点变为前置节点
            pre = currentNode;
            // 重新创建一个当前节点，关联前置节点
            currentNode = new Node();
            currentNode.pre = pre;
            // 设置值
            currentNode.value = value;
        }
    }

    // 弹出栈顶元素
    public T pop(){
        if (currentNode==null) {
            return null;
        }

        // 减少长度
        count(false);

        T value = (T) currentNode.value;
        currentNode = currentNode.pre;
        return value;
    }

    // 对栈长度的计算
    public void count(boolean isAdd){
        if (isAdd){
            size++;
        }else if (!isAdd && size>0){
            size--;
        }
    }


    // 获取栈的长度
    public int size(){
        return size;
    }

    // 判断栈是否为空
    public boolean isEmpty(){
        return size==0;
    }

    // 重写toString方法，打印栈中内容
    @Override
    public String toString() {
        if (currentNode==null){
            return "[]";
        }
        Node node = currentNode;
        StringBuilder builder = new StringBuilder();
        builder.append("[");
        while (node!=null){
            builder.append(node.value).append(",");
            node = node.pre;
        }
        builder.deleteCharAt(builder.length()-1);
        builder.append("]");
        return builder.toString();
    }

   }

   // 节点对象，存的值和前置节点
   class Node<T> {
    T value;
    Node<T> pre;
   }
   ```

   测试结果，可以看到确实是先进后出的顺序。

   ```
    public static void main(String[] args){
        SimpleStack<String> stack = new SimpleStack<String>();

        for(int i=0;i<10;i++){
            stack.push(i+"str");
        }

        System.out.println("栈长度-" + stack.size());
        System.out.println("栈是否为空-" + stack.isEmpty());

        for(int i =0;i<10;i++){
            System.out.println(stack.pop());
        }
        System.out.println("栈长度-" + stack.size());
        System.out.println("栈是否为空-" + stack.isEmpty());
    }

    栈长度-10
    栈是否为空-false
    9str
    8str
    7str
    6str
    5str
    4str
    3str
    2str
    1str
    0str
    栈长度-0
    栈是否为空-true
   ```

3. Java中自带的栈

   Java中本身就已经有实现的Stack栈了，Stack继承自Vector，相比之下自带的栈功能就更加丰富。

   ```
   Stack<Integer> stack = new Stack<Integer>();
   stack.add(1);
   stack.push(1);
   stack.add(1,2);
   stack.addAll(new ArrayList<>());
   ```