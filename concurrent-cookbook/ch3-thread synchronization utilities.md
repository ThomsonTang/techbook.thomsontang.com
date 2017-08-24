
## 术语卡——thread synchronization
- 术语：thread synchroziation
- 印象：
	- 线程同步的基本机制：Basic Thread Synchronization
		- the `synchronized` keyword
		- the `Lock` interface and its implementations: `ReentrantLock`、`ReentrantReadWriteLock.ReadLock`、`ReentrantReadWriteLock.WriteLock`
	- 线程同步工具：Thread Synchronization Utilities
		- `Semaphores`: 信号量是一种计数器，用于控制对一个或多个共享资源的访问。
		- `CountDownLatch`: 闭锁机制允许一个线程一直等待至若干操作结束。
		- `CyclicBarrier`: 栅栏允许多个线程在一个共同点上的同步。
		- `Phaser`: Phaser将并发任务分为多个阶段并控制器执行，所有线程必须在进入下一阶段时完成当前阶段的工作。
		- `Exchanger`: Exchanger在两个线程之间提供了一个数据交换的点。


## 术语卡——闭锁
- 术语：CountDownLatch
- 印象：`CountDownLatch`主要用来实现让线程等待直到若干事件或操作发生以后再执行。
	- `CountDownLatch`类有三个基本的元素：
		- 初始值，这个用来表示CountDownLatch类要等待的事件或操作的数量。
		- `await()`方法，这个方法是由等待操作事件结束后再执行的线程来调用的。
		- `countDown()`方法，这个方法是操作事件在完成它们的执行后调用的。
	- `CountDownLatch`一旦创建就无法再次初始化或者修改其内部的计数器，唯一能做的就是调用`countDown()`方法来改变它的值。当这个计数器的值达到`0`的时候，所有调用了`await`的方法都将立即返回，此后再调用`countDown()`将不会有任何作用。
	- 相对于其他同步方法，`CountDownLatch`有两点区别需要注意：
		- `CountDownLatch`机制并不是用来保护共享资源或临界区的。它是用来同步那些有不同任务要执行的一个或多个线程。
		- `CountDownLatch`只能使用一次，一旦用完就不能再使用了，只能重新创建一个。
- 例子：「视频会议参会」的例子，`VideoConference.java`

