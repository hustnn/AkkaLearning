怎样为股票聚类呢？想了一种算法，先试试

Maven 3.0不鼓励systemPath，如果想要依赖一个本地的jar包，最好的办法是起一个单独的project，里面只做一件事：把放在lib目录下的jar用maven-install-plugin安装到本地仓库。Project的标识可以与install时的配置完全一样。

关于local的jar，最终的方案是建了一个统一的libs.local项目，专门用来自动安装这些jar到本地仓库。例子可见：https://github.com/dcaoyuan/nbscala/tree/master/libs.local

用Scala+JavaFX画曲线还是很方便的，这是一段代码。源码在：

Scala 如果object中的一个val量在初始化时不是简单赋值，而是需要做一些操作，比如从数据库加载东西，那么一定把它定义为lazy val，这样的object才可以任意引用。

Chana 能做什么。以前，计算 x+y，你要写 var x=1, var y=2, var z=x+y，然后得到结果 3。但当 x, y 变化时，需要把这个步骤用新的值重算一遍，这需要你自己处理。而在 Chana 中，z 实时反映 x 或 y 的变化，而且，这样的 x, y 可以是成千上万上亿个，可以是嵌套数据结构，可以分布在上千台机器上。

Question 1: 
Compared with traditional SQL query. It seems that the query has status and the query result can be monitored continuously and the result is updated once some monitored values change.

Is there any differences when doing the query optimization on the Query plan? What is the special aspect for JPQL on Chana?

Ans:
The data are stored as Avro object, each avro record is an actor. When a JPQL query is applied to cluster, it will be parsed, and the applicable part (such as where clause, select related fields) will be evaluated by each record (actor) itself, and then push the minimal projection to aggregator (a singleton actor in cluster), then is evaluated by aggregator.

It's still under heavy developing, there are lots of optimization works to be done. But, you may have noticed, the README gives an example that should have worked.



回复我的评论：每条record是一个actor，是把一些hot的record放在memory里面，cold的record利用akka persistent到disk么？
是