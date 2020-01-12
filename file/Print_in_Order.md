## Print in Order

Suppose we have a class:
```
public class Foo {
  public void first() { print("first"); }
  public void second() { print("second"); }
  public void third() { print("third"); }
}
```
The same instance of Foo will be passed to three different threads. Thread A will call first(), thread B will call second(), and thread C will call third(). Design a mechanism and modify the program to ensure that second() is executed after first(), and third() is executed after second().

 

- Example 1:

  Input: [1,2,3]

  Output: "firstsecondthird"

  Explanation: There are three threads being fired asynchronously. The input [1,2,3] means thread A calls first(), thread B calls second(), and thread C calls third(). "firstsecondthird" is the correct output.

- Example 2:

  Input: [1,3,2]

  Output: "firstsecondthird"

  Explanation: The input [1,3,2] means thread A calls first(), thread B calls third(), and thread C calls second(). "firstsecondthird" is the correct output.
 

- Note:

  We do not know how the threads will be scheduled in the operating system, even though the numbers in the input seems to imply the ordering. The input format you see is mainly to ensure our tests' comprehensiveness.

---

## 解题思路

这道题目的意思给定了一个类，类里面有三个方法，分别打印first、second和third，这三个方法都是异步子线程执行的，会随机顺序调用这三个办法，要求确保执行顺序一定是first、second和third。

- 加锁使用wait和notifyAll

  首先因为这三个方法是乱序调用，而且中途还会互相抢夺执行权，所以要保证顺序的话就要判断如果不是当前要求的顺序的方法，就调用wait方法让它等待，所以这里定义了一个flag标记来约束执行的顺序。

  因为可以先调用了后面的second等方法，所以如果first没调用的时候second和third都会等待，这个时候first再执行完需要修改标记并唤醒等待的second和third，让他们俩再去重复上面的步骤，直到最后都按顺序执行完，因为wait和notifyAll方法都是需要有锁的情况下执行，所以方法都需要加synchronized关键字来加锁。

  ```
  class Foo {

	int flag = 1;

	public Foo() {

	}

	public synchronized void first(Runnable printFirst) throws InterruptedException {
		while (flag != 1) {
			wait();
		}
		// printFirst.run() outputs "first". Do not change or remove this line.
		printFirst.run();

		flag = 2;
		notifyAll();
	}

	public synchronized void second(Runnable printSecond) throws InterruptedException {
		while (flag != 2) {
			wait();
		}
		// printSecond.run() outputs "second". Do not change or remove this line.
		printSecond.run();
		flag = 3;
		notifyAll();
	}

	public synchronized void third(Runnable printThird) throws InterruptedException {
		while (flag != 3) {
			wait();
		}
		// printThird.run() outputs "third". Do not change or remove this line.
		printThird.run();
		flag = 0;
	}

  }
  ```