
## 术语卡
- 术语：对象的状态（the state of an object)
- 印象：我们常说的对象的状态一般是指**保存在状态变量（比如实例变量或静态域）中的数据**。当然了，某个对象的状态可能包含了其他对象的实例变量或静态域。而且一个对象的状态还包括那些**能影响它的外部可见行为的数据**。
- 例子：`HashMap`对象的状态不仅存储在`HashMap`对象本身，而且还存储在许多的`Map.Entry`对象中。
- 出处：《Java Concurrency in Practice》第二章

## 术语卡
- 术语：共享和可变（Shared and Immutable）
- 印象：共享意味着一个变量可以被多个线程同时访问；可变意味着变量的值在其生命周期内是可以变化的。这指的是在程序中使用对象的方式，而不是对象所实现的功能。
- 例子：
	- 类的静态属性（static fields）可以被该类的所有对象实例共享
	- Java中不可变一般都是通过关键字`final`来修饰的。Java中的`String`类的对象就是典型的不可变对象。
- 出处：《Java Concurrency in Practice》第二章

## 技巧卡
- 如果多个线程同时访问同一个可变状态的变量，但是没有使用合适的同步，那么程序就有可能出错。有三种办法可以解决：
	- 不要在多个线程之间共享这个状态变量。
	- 把状态变量修改为不可变的。
	- 访问这个状态变量时使用同步。
- 在设计线程安全的类时，良好的面向对象的技术——封装性、不可变性以及明确的不变性规范等都是很好的辅助手法。

## 术语卡
- 术语：线程安全性
- 印象：线程安全的关键在于**正确性**，具体讲就是，在多线程的环境下，类表现出的行为与其规范是完全一致的。当多个线程访问某个类的时候，这个类总能表现出正确的行为，那么它就是线程安全的。
	> 当多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在主调代码中不需要任何额外的同步或协同，这个类都能表现出正确的行为，那么就将这个类称之为线程安全的。
	如果某个类在单线程环境下都是不正确的，那么它肯定不是线程安全的。
	> 线程安全的类本身在内部封装了必要的同步机制，因此客户端无须进一步采取同步措施。
- 例子：一个无状态的Servlet

	```java
	@ThreadSafe
	public class StatelessFactorizer implements Servlet {
		public void service(ServletRequest req, ServletResponse resp) {
			BigInteger i = extractFromRequest(req);
			BigInteger[] factors = factor(i);
			encodeIntoResponse(resp, factors);
		}
	}
	```
	> 无状态对象一定是线程安全的。大多数Servlet都是无状态的，这大大降低了在实现Servlet线程安全性时的复杂性，只有当Servlet在处理请求时需要保存一些信息，线程安全性才会成为一个问题。

## 术语卡
- 术语：竞态条件（Race Condition）
- 印象：在并发编程中，由于不恰当的执行时序而出现不正确的结果，称之为竞态条件。
- 例子：
	- 有状态的Servlet，其中的`++count`是一个`Read-Modify-Write`的操作序列，这个操作不是**原子性**的，是**非线程安全**的。

		```java
		@NotThreadSafe
		public class UnsafeCountingFactorizer implements Servlet {
			private long count = 0;
			public long getCount() {
				return count;
			}
			public void service(ServletRequest req, ServletResponse resp) {
				BigInteger i = extractFromRequest(req);
				BigInteger[] factors = factor(i);
				++count;
				encodeIntoResponse(resp, factors);
			}
		}
		```
	- `Check-then-Act`，「先检查后执行」的操作序列，这是延迟初始化中常见的一种竞态条件。
		```java
		@NotThreadSafe
		public class LazyInitRace {
				private ExpensiveObject instance = null;
				public ExpensiveObject getInstance() {
					if (instance == null) {
						instance = new ExpensiveObject();
					}
					return instance;
				}
		}
		```
	- 与大多数并发错误一样，竞态条件并不总是会产生错误，还需要某种不恰当的执行时序。
- 出处：《Java Concurrency in Practice》第二章——线程安全性——原子性


## 术语卡
- 术语：复合操作
- 印象：「竞态条件」部分提到的`Read-Modify-Write`和`Check-then-Act`两种操作序列本质上都应该是**原子性**的，这种操作统称为**复合操作**，即**包含了一组以原子方式执行的操作以确保线程安全性**。
- 例子：
	- **加锁机制**：这是Java用于确保原子性的内置机制。
	- 使用现有的线程安全的类：比如原子操作类`AtomicInteger`、`AtomicLong`，或线程安全容器类`ConcurrentHashMap`等。
		```java
		@ThreadSafe
		public class CountingFactorizer implements Servlet {
				private final AtomicLong count = new AtomicLong(0);
				public long getCount() {
					return count.get();
				}
				public viod service(ServletRequest req, ServletResponse resp) {
					BigInteger[] i = extractFromRequest(req);
					BigInteget[] factors = factor(i);
					count.incrementAndGet();
					encodeIntoResponse(resp,factors);
				}
		}
		```
	- 在实际编程中，应尽可能地使用现有的线程安全对象，比如`AtomicLong`等。

## 术语卡
- 术语：内置锁
- 印象：
	- Java提供了一种内置锁的机制来实现原子操作，这就是我们熟悉的**同步代码块**，用关键字`synchronized`来修饰。
	- 每个Java对象都可以用作一个实现同步的锁，这些锁被称为**内置锁（intrinsic lock）**。
	- 获得内置锁的唯一方法就是**进入由这个锁保护的同步代码块或方法**。
	- 内置锁是一种互斥锁，这就意味着同一时刻最多只能有一个线程持有这个锁。当线程B试图获取一个由线程A持有的锁时，它必须等待或者阻塞，直到线程A释放了这个锁。
	- 受内置锁保护的代码块，因为同一时刻只能有一个线程访问，所以这种操作是原子性的，是线程安全的。
	- 内置锁虽然保证了线程安全性，但是有时候会带来性能问题。
- 例子：
	- 同步代码块：
		```java
		synchronized(lock) {
				//访问或修改由锁保护的共享状态
		}
		```

- 同步方法：
	```java
	public synchronized void method() {
			//访问或修改由锁保护的共享状态
	}
	```


## 可重入
- 术语：可重入
- 印象：
	- 当某个线程视图访问一个其他线程持有的锁时，就会被阻塞。但内置锁具有可重入性，所以一个线程试图获取一个已经由他自己持有的锁时，就会成功获取到。这就是内置锁的**可重入性**。
	- 「重入」意味着获取锁的操作粒度是「线程」，而不是「调用」。