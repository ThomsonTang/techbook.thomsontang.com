
## 反常识卡
- 常识：多线程能够最大的提高系统的资源利用率和吞吐量，而且在某些情况下还能简化系统的复杂度。线程通常具有如下几个优点：
	- **Exploiting Multiple Processor**: 现代计算机大多都是多核的，因此作为操作系统基本调度单位的线程能够最大程度的保证计算机的每个核心都能有事可干，从而发挥计算机的多核处理能力。
	- **Simplicity of Modeling**：有了多线程，我们在为某些复杂的系统或者应用建模时，就能将其功能拆分为若干个任务，每个任务可以有独立的线程来完成，最后将结果汇总。我平常使用的许多框架正式利用多线程解决了**请求管理**、**线程创建**、**负载均衡**等不同的细节问题。
	- **Simplified Handling of Asynchronous Events**: 对于异步处理事件来说，多线程能使各个事件之间相互独立，各行其事，而不会出现互相阻塞的情况。

- 反常识：我们除了关心线程带给我们的好处之外，在使用多线程的时候，还要注意它所带来的风险，毕竟多线程编程增加了开发的复杂度。多线程还会带来如下的风险：
	- 安全性问题
	- 活跃性问题
	- 性能问题
- 例子：
	- **Safety Hazards**：线程安全是一个非常复杂的问题，在没有足够同步的情况下，多线程中的操作执行顺序是不可预测的，常常会产生一些奇怪的结果。
		- Non-thread-safe Sequence Generator:

		```java
		@NotThreadSafe
		public class UnsafeSequence{
			private int value;
			
			// Returns a unique value.
			public int getNext() {
				return value++;
			}
		}
		```

		上面的例子的问题主要在于**如果两个线程执行的时机不对，那么在调用 `getNext()` 方法后会得到相同的值**。这种是一种很常见的并发安全问题，称之为**竞态条件（Race Condition）**。在多线程环境下，`getValue()` 是否会返回唯一的值，取决于运行时对线程中操作的交替执行方式，但这并不是我们想要看到的结果。想要多线程程序的行为可以预测，我们就必须对**共享变量**的访问操作进行一定的协同，使得他们不会互相干扰。幸运的是，**Java提供了各种同步机制来协调这种访问**。
		- Thread-safe Sequence Generator:

			```java
			@ThreadSafe
			public class Sequence {
					private int value;
					
					public synchronized int getNext() {
						return value++;
					}
			}
			```

将取值方法 `getNext()` 改为同步方法以后就可以修复之前的线程安全性问题了。

- **Liveness Hazards**: