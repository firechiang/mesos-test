#### 中文文档: https://docs.huihoo.com/apache/mesos/mesos-cn/
##### 理论
```bash
Mesos Master 通过 Zookeeper 搭建 HA，理论上只有一台提供服务;
Mesos Slave 运行在物理机或者虚拟机之上，用来跑具体任务或服务;
Mesos Slave 启动时都会注册到 Mesos Master，由 Mesos Master 协调并且确定 Mesos Slave 资源情况 <可用资源>;

第一级调度：Mesos Master 调度 Mesos Slave 执行具体作业;
Framework 由调度器 和 执行器组成 被注册到 Mesos 一使用 Mesos 集群之中的资源;
```

##### 调度过程
```bash
1 Mesos Slave 定期向 Mesos Master 汇报当前机器的空闲资源 比如：4 个CPU，4G 内存;
2 Masos Master 向 Framework 发送资源邀约<当前可用资源>，描述了 Masos Slave 所有的可用资源。比如：4 个CPU，4G 内存;
3 Framework 调度器向 Mesos Master 请求要在 Masos Slave 运行几个任务，并且发送任务执行器<执行器可以理解为一段代码>，第一个任务需要 2个CPU，2G内存；第二个任务需要 1个CPU，1G内存;
4 Mesos Master 向 Mesos Slave 下发任务并且分配适当的资源给 Framework 的执行器，去执行这几个任务<每个任务是隔离开来的>;
```

##### 资源邀约和分配策略
```bash
每次资源邀约都会包一个含 Mesos Slave 上的可用CPU和内存等资源列表，Mesos Master 会提供资源给 Framework，分配策略对所有的 Framework 都是通用的;
Framework 也可以拒绝资源邀约如果不满足要求的话; 如果是这样的话 资源邀约可以随机发送给其它的 Framework;
```
