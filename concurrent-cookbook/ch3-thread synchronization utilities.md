
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
- 例子：
	- 