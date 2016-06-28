减小协调成本
======================================
> 请求原谅，总是比得到许可更容易 -- Grace Hopper [1] 

**我要怎么做才能在设计微服务的时候数据耦合最小化，但是确实有些场景需要各个微服务之间协调数据?**

这是我们预料之中的，然而并不是设计中的一个错误。许多用微服务构建的系统都有数据协调的场景。幸运的是，你在这个设计阶段加入你需要的协调，而不是一开始耦合后续才删除--这比前者困难得多。

确实有合理的方式在可扩展和高可用的模式下协调更改数据，但是这要求你所操作的数据是可组合的。

*可组合性*在这里是指更必数据的产生能够不暂停他们(或者你自己)，不需要等待协调者完成。

接下来我将讨论如何通过通信协议来解决这个问题，这些技术包括业务补偿(`Apology-Orient Programming`)，EDA和ACID 2.0。

业务补偿的思想建立在原谅比较认可容易这个道理上，当你不能协调(以及不确定某些事情)，然后作一个有根据的推测，一个还能把握住的打赌，如果你担心，你会道歉和做一些补偿的动作。

这种方法很符合现实，人们往往都这个来协调的。其他的例子包括ATM机--网络断开的时候取钱，后续再减掉你的金额。还有飞机票超卖--过发放代金券给来讨好用户。

这个模型用EDA也非常适合，充分利用异步消息传递和事件溯源。这个模型对于区分命令和事件是非常重要的，命令代表着意图影响操作的那边--就是Pat Hellend所说的"未来的希望"。然而事件代表着已经发生的事实--历史是通过当前的事件来重演。







----------------
[1]. [Grace Hopper](https://en.wikiquote.org/wiki/Grace_Hopper), 美国海军准将（Rear admiral）及计算机科学家，世界最早一批的程序员之一，也是最早的女性程序员之一,当她开始自行开发编译器时，并没有得到公司高层的许可。她说:"请求原谅，总是比得到许可更容易。"。 
[2]. Pat Helland 并没有用Apology-Oriented Programming这个词，但是他在博客上[记忆,猜测和道歉]()介绍过这个的一般含义。
