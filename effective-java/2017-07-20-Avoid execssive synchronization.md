
# 反常识卡
- 反常识：在某些场景下，过度的同步会导致性能下降、死锁甚至出现不确定的行为。
- 常识：多线程环境中，要警惕不充分的同步导致的危险，所以尽可能的保证同步。
- 例子：
	- **为了避免活跃性和安全性错误，在同步方法和同步块中，不要将控制权交给客户端**。换句话说，在一个同步块中，不要调用那些被设计成可覆写的方法，或者是那些由客户端以**函数对象**的形式提供的方法。从同步块自己的角度来看，这些方法都是「外来的」。类通常对这种方法一无所知或者没有任何的控制权。具体参看[Java-Review/ObservableSetTest.java · ThomsonTang/Java-Review]()(https://github.com/ThomsonTang/Java-Review/blob/master/effective-java/src/test/java/effective/concurrency/item67/ObservableSetTest.java)这个例子。

