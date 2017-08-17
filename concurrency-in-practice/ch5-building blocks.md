
## 技巧卡
> 在实践中，**委托**是创建线程安全类的一种非常有效的策略：**只需要让现有的线程安全的类来管理所有的状态即可**。java平台本身提供了丰富的并发构建模块，比如常见的线程安全的集合，能够协调多个线程相互协作的同步工具类等。

## 术语卡——同步集合
- 术语：Synchronized Collections
- 印象：诸如`Vector`、`Hashtable`之类的比较「古老」的java同步集合类。这些同步的包装类都是通过`Collections.synchronizedXXX`工厂方法来创建的。**这些类是通过封装他们的状态并且同步每一个公共方法来实现同步的，因此每次只能有一个线程访问集合的状态**。
- 例子：
	- `Vector`
	- `Hashtable`

## 反常识卡—— Problems with Synchronized Collections
- 常识：同步集合类是线程安全。
- 反常识：
	- 虽然同步集合类是线程安全的，但是有时候还是需要额外的客户端加锁来保护复合操作。集合类中常见的复合操作包括：迭代、获取下一个元素以及有条件的操作，例如：先判断有没有元素，没有再添加。**在同步集合类中，这些复合操作在没有客户端加锁的情况下是线程安全的，但是当其他线程并发的修改集合类是，可能会出现与预期不一致的结果**。
	- 虽然加锁可以防止迭代器抛出`ConcurrentModificationException`，但前提是需要在所有对共享容器进行迭代的地方加锁，包括那些隐式的地方。
	- 同步集合类是**通过将所有的对容器状态的访问串行化，来实现线程安全性的**。这种方法的代价的是并发性严重降低，多线程访问容器时，吞吐量严重降低。
- 例子：
	- Compound Actions on a `Vector` that may Produce Confusing Results
		[CompoundActionOnVector.java][1]
	- Iterators and `ConcurrentModificationException`：循环遍历同步集合对象时，如果有其他线程也在同时操作这个集合对象，那么就有可能引发`ConcurrentModificationException`。
	- Hidden Iterators: 
		- 例如，某些同步集合类的`toString()`方法内部实际上执行了迭代遍历
		- 容器的`hashCode()`和`equals()`等方法也会间接地执行迭代操作。
		- 同样，`containsAll()`、`removeAll()`、`retainAll()`等，以及把容器作为参数的构造函数，都会对容器进行迭代
		所有这些间接操作都有可能抛出`ConcurrentModificationException`。

## 术语卡——并发容器
- 术语： 并发容器（Concurrent Collections）
- 印象：
	- 并发容器类是专门为多个线程并发访问设计的。
	- 并发容器类不仅可以替代同步容器类，而且具有更好的并发特性，性能也要比同步容器好很多。
- 例子：
	- `ConcurrentHashMap`
	- `CopyOnWriteArrayList`
	- Java 5.0 新增的`Queue`和`BlockingQueue`:
		- Queue用来临时保存一组待处理的元素，Queue的操作不会阻塞，如果队列为空，那么获取元素的操作将返回空值
		- BlockingQueue扩展了Queue，增加了可阻塞的插入和获取操作。如果队列为空，获取元素的操作将一直阻塞，直到队列中出现一个可用的元素；如果队列已满（有界队列），那么插入元素的操作将一直阻塞，直到队列中出现可用的空间。

## 术语卡——ConcurrentHashMap
- 术语：ConcurrentHashMap
- 印象：
	- 我们知道同步容器类在执行每一个操作期间都会持有一个锁，这通常会导致并发效率很低，所以ConcurrentHashMap采用了一种完全不同的加锁策略，称之为**分段锁**，这是一种粒度更精细的加锁机制。这种机制下，任意多的读线程都可以并发访问Map，读取操作的线程和写入操作的线程也可以并发访问Map，甚至一定数量的写线程都可以并发修改Map。
	- ConcurrentHashMap提供的迭代器不会抛出`ConcurrentModificationException`，因此在迭代过程中不需要加锁。
	- 由于ConcurrentHashMap不能被加锁来执行独占访问，所以我们无法使用客户端加锁来创建新的原子操作。



[1]:	https://gist.github.com/ThomsonTang/4751d33d55e6c8b819f885bf06ecd3b0