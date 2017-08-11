
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
- 反常识：虽然同步集合类是线程安全的，但是有时候还是需要额外的客户端加锁来保护复合操作。集合类中常见的复合操作包括：迭代、获取下一个元素以及有条件的操作，例如：先判断有没有元素，没有再添加。**在同步集合类中，这些复合操作在没有客户端加锁的情况下是线程安全的，但是当其他线程并发的修改集合类是，可能会出现与预期不一致的结果**。
- 例子：
	- Compound Actions on a `Vector` that may Produce Confusing Results
	<script src="https://gist.github.com/ThomsonTang/23945ef7d7a5741b4ff1beda73cf0408.js"></script>


