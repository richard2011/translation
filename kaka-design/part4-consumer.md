消费者
=======================================
Kafka消费者通过向Broker的Leader分区执行『拉』请求来进行消费。消费者在每次请没地方指定在log里的offset然后接收一批从这个position开始的log。因些消费者可以完全这个position，如果需要还可以重新消费数据。

### Push vs. pull

