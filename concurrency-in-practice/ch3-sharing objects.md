
## 反常识卡——关键字`synchronized`的作用
- 常识： 同步代码块和同步方法可以确保以原子的方式执行操作，所以有人认为关键字`synchronized`只能用于实现原子性或者「临界区」（Critical Section）。
- 反常识：同步其实还有另外一个重要的特性，即**内存可见性（Memory Visiblity）**。
	- 可见性：在多线程编程中，我们不仅希望防止某个线程在使用对象状态的同时被另一个线程修改该状态，而且希望确保当一个线程修改了对象状态后，其他线程能够看到这种状态的变化。
	- 重排序：在没有同步的情况下，编译器、处理器以及运行时等都可能对操作的执行顺序进行一些意想不到的调整，达到优化处理的目的。但是这就导致我们在判断内存操作的执行顺序时，很难得出正确的结论。
	- 不过幸运的是，有一种简单的方法可以避免这些复杂的问题：**只要有数据在多个线程之间共享，就使用正确的同步**。
- 例子：在没有同步的情况下共享变量：
	```java
	public class NoVisibility {
			private static boolean ready;
			private static int number;

			private static class ReaderThread extends Thread {
				public void run() {
					while(!ready) {
						Thread.yield();
					}
					System.out.println(number);
				}
			}
			
			public static void main(String[] args) {
				new ReaderThread().start();
				number = 42;
				ready = true;
			}
	}
	```
	上面的示例程序看起来会输出`42`，但事实上很可能输出`0`，或者根本无法终止。这是因为在代码中没有使用足够的同步机制，因此无法保证主线程写入的`ready`值和`number`值对于读线程来说是可见的。
