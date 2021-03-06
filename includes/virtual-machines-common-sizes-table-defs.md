
## <a name="size-table-definitions"></a>大小表定义

* 存储容量的单位为 GiB 或 1024^3 字节。 比较以 GB（1000^3 字节）为单位的磁盘和以 GiB（1024^3 字节）为单位的磁盘时，请记住以 GiB 为单位的容量数显得更小。 例如，1023 GiB = 1098.4 GB
* 磁盘吞吐量的单位为每秒输入/输出操作数 (IOPS) 和 MBps，其中 MBps = 10^6 字节/秒。
* 数据磁盘可以在缓存或非缓存模式下运行。  对于缓存数据磁盘操作，主机缓存模式设置为 **ReadOnly** 或 **ReadWrite**。  对于非缓存数据磁盘操作，主机缓存模式设置为 **None**。
* 最大网络带宽是每个 VM 类型分配并分配的最大聚合带宽。 最大带宽提供选择正确 VM 类型的指导原则，以确保有适当的网络容量可用。 在低、中、高和极高之间切换时，吞吐量将随之增加。 实际的网络性能将取决于许多因素，包括网络和应用程序负载，以及应用程序的网络设置。