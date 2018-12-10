
.. _throughput_benchmark:

============
消息吞吐测试
============

EMQX消息吞吐测试组合条件：

|  QoS    |         Payload       | PUB连接 x Fan-In | SUB连接 X Fan-Out |  实际Fan-In  |  实际Fan-Out  |  背景连接   |
|---------|-----------------------|-----------------|-------------------|-------------|---------------|-----------|
| 0\1\2   | 64B\256B\1k\10K\100k  |    C x Msg/s    |     C x Msg/s     |  Msg/s, Bps |  Msg/s, Bps   |    100K   |

参数说明:

|  参数     |   说明                |
|----------|-----------------------|
|  C       |   Connection连接数     |
|  Msg/s   |   每秒消息数量          |
|  Bps     |   网络吞吐(字节/秒)     |

.. NOTE:: 吞吐测试在青云北京三区VPC进行，节点之间的网络带宽平均限制为1Gbps，峰值限制为2Gbps。

-------------------
QoS0 Fan-In消息吞吐
-------------------

测试客户端到EMQ服务器间的QoS0消息吞吐:

| 组合场景ID              |  QoS  |  Payload  |  PUB连接 X Fan-In  |  SUB连接 X Fan-Out  |  Fan-In(平均吞吐)  |  背景连接   |
|------------------------|-------|-----------|-------------------|---------------------|-------------------|------------|
| qos0-p256-40K-0        |  0    |  256      |  4K X 10          |  0                  |     38221         |  100K      |
| qos0-p1K-30K-0         |  0    |  1K       |  3K X 10          |  0                  |     28552         |  100K      |
| qos0-p10K-10K-0        |  0    |  10K      |  1K X 10          |  0                  |     9527          |  100K      |
| qos0-100K-1K-0         |  0    |  100K     |  1K X 1           |  0                  |     966           |  100K      |


.. NOTE:: 1. 测试主机的内核TCP协议栈参数: http://emqx.com/docs/v3/tune.html#tcp

资源占用报告:


|  组合场景ID              | 网络吞吐(Avg/Max Bps) | CPU负载(ShortLoad Max) | CPU(user/sys)  | Memory(Max) |
|-------------------------|---------------------|------------------------|----------------|-------------|
|  qos0-p256-40K-0        | _/14.70M            |           8            | 不超过55% / 25% | 1.95G       |
|  qos0-p1K-30K-0         | _/33.80M            |           7            | 不超过48% / 20% | 1.90G       |
|  qos0-p10K-10K-0        | _/102.20M           |           6            | 不超过36% / 14% | 1.89G       |
|  qos0-100K-1K-0         | _/102.60M           |           7            | 不超过29% /  7% | 2.38G       |


qos0-p256-40K-0 EMQX服务器资源监控：

![image](./_static/images/qos0-p256-40K-0.png)

qos0-p1K-30K-0 EMQX服务器资源监控：

![image](./_static/images/qos0-p1K-30K-0.png)

qos0-p10K-10K-0 EMQX服务器资源监控：

![image](./_static/images/qos0-p10K-10K-0.png)

qos0-100K-1K-0 EMQX服务器资源监控：

![image](./_static/images/qos0-100K-1K-0.png)


--------------------
QoS0 Fan-Out消息吞吐
--------------------

|  组合场景ID              |  QoS  |  Payload  |  PUB连接 X Fan-In  |  SUB连接 X Fan-Out  |  Fan-Out(平均吞吐) |  背景连接   |
|-------------------------|-------|-----------|-------------------|---------------------|------------------|------------|
|  qos0-p256-4-40K        |  0    |  256      |  4 X 1            |  10K X 4            |     36075        |  100K      |
|  qos0-p1K-3-30K         |  0    |  1K       |  3 X 1            |  10K X 3            |     28967        |  100K      |
|  qos0-p10K-1-10K        |  0    |  10K      |  1 X 1            |  10K X 1            |     8829         |  100K      |
|  qos0-p100K-1-1K        |  0    |  100K     |  1 X 1            |  1K X 1             |     997          |  100K      |


资源占用报告:


|  组合场景ID              | 网络吞吐(Avg/Max Bps) | 负载(Load) | CPU(user/sys) | Memory(Avg/Max) |
|-------------------------|----------------------|-----------|---------------|-----------------|
|  qos0-p256-4-40K        | _/24.08M             |  8        | 不超过40% /17% |   4.32G         |
|  qos0-p1K-3-30K         | _/42.80M             |  8        | 不超过35% /18% |   3.53G         |
|  qos0-p10K-1-10K        | _/112.30M            |  6        | 不超过23% /11% |   3.77G         |
|  qos0-p100K-1-1K        | _/106.00M            |  2        | 不超过18% /15% |   2.98G         |

qos0-p256-4-40K  EMQX服务器资源监控：

![image](./_static/images/qos0-p256-4-40K.png)

qos0-p1K-3-30K  EMQX服务器资源监控：

![image](./_static/images/qos0-p1K-3-30K.png)

qos0-p10K-1-10K  EMQX服务器资源监控：

![image](./_static/images/qos0-p10K-1-10K.png)

qos0-p100K-1-1K  EMQX服务器资源监控：

![image](./_static/images/qos0-p100K-1-1K.png)


-------------------
QoS1 Fan-In消息吞吐
-------------------

|  组合场景ID              |  QoS  |  Payload  |  PUB连接 X Fan-In  |  SUB连接 X Fan-Out  |  Fan-In(平均吞吐) |  背景连接  |
|-------------------------|-------|-----------|-------------------|---------------------|-----------------|-----------|
|  qos1-p256-30K-0        |  1    |  256      |  3K X 10          |  0                  |    28014        |  100K     |
|  qos1-p1K-20K-0         |  1    |  1K       |  2K X 10          |  0                  |    18674        |  100K     |
|  qos1-p10K-5K-0         |  1    |  10K      |  1K X 5           |  0                  |    4714         |  100K     |


资源占用报告:


|  组合场景ID              | 网络吞吐(Avg/Max Bps) | 负载(Load) | CPU(user/sys) | Memory(Avg/Max) |
|-------------------------|---------------------|------------|---------------|-----------------|
|  qos1-p256-30K-0        | _/12.49M            |      7     | 不超过63% /25% |  _/1.90G        |
|  qos1-p1K-20K-0         | _/23.41M            |      7     | 不超过40% /22% |  _/1.91G        |
|  qos1-p10K-5K-0         | _/50.16M            |      4     | 不超过24% /12% |  _/1.90G        |


qos1-p256-30K-0 EMQX服务器资源监控：

![image](./_static/images/qos1-p256-30K-0.png)

qos1-p1K-20K-0 EMQX服务器资源监控：

![image](./_static/images/qos1-p1K-20K-0.png)

qos1-p10K-5K-0 EMQX服务器资源监控：

![image](./_static/images/qos1-p10K-5K-0.png)


--------------------
QoS1 Fan-Out消息吞吐
--------------------


|  组合场景ID              |  QoS  |  Payload  |  PUB连接 X Fan-In  |  SUB连接 X Fan-Out  |  Fan-Out(平均吞吐)  |  背景连接   |
|-------------------------|-------|-----------|-------------------|---------------------|-------------------|------------|
|  qos1-p256-4-40K        |  1    |  256      |  4 X 1            |  10K X 4            |   28802           |  100K      |
|  qos1-p1K-3-30K         |  1    |  1K       |  3 X 1            |  10K X 3            |   22903           |  100K      |
|  qos1-p10k-1-5K         |  1    |  10K      |  1 X 1            |  5K X 1             |   4210            |  100K      |


资源占用报告:


|  组合场景ID              | 网络吞吐(Avg/Max Bps) | 负载(Load) | CPU(user/sys)  | Memory(Avg/Max) |
|-------------------------|---------------------|------------|----------------|-----------------|
|  qos1-p256-4-40K        | _/15.70M            |   8        | 不超过59% / 30% | _/3.70G         |
|  qos1-p1k-3-30K         | _/33.60M            |   8        | 不超过52% / 28% | _/3.62G         |
|  qos1-p10k-1-5K         | _/49.40M            |   6        | 不超过25% / 20% | _/3.18G         |


qos1-p256-4-40K  EMQX服务器资源指标监控：

![image](./_static/images/qos1-p256-4-40K.png)


qos1-p1k-3-30K  EMQX服务器资源指标监控：

![image](./_static/images/qos1-p1k-3-30K.png)


qos1-p10k-1-5K  EMQX服务器资源指标监控：

![image](./_static/images/qos1-p10k-1-5K.png)


-------------------
QoS2 Fan-In消息吞吐
-------------------


|  组合场景ID              |  QoS  |  Payload  |  PUB连接 X Fan-In  |  SUB连接 X Fan-Out  |  Fan-In(平均/峰值)  |  背景连接   |
|-------------------------|-------|-----------|-------------------|---------------------|-------------------|------------|
|  qos2-p256-20K-0        |  2    |  256      |  4k X 5           |  0                  |  17548            |  100K       |
|  qos2-p1K-10K-0         |  2    |  1K       |  2k X 5           |  0                  |  9520             |  100K       |
|  qos2-p10K-3k-0         |  2    |  10K      |  600 X 5          |  0                  |  2845             |  100K       |


资源占用报告:


|  组合场景ID              | 网络吞吐(Avg/Max Bps) | 负载(Load) | CPU(user/sys) | Memory(Avg/Max) |
|-------------------------|----------------------|-----------|---------------|-----------------|
|  qos2-p256-20K-0        | _/10.88M             |    8      | 不超过60%/26%  | _/2.02G         |
|  qos2-p1k-10K-0         | _/13.18M             |    7      | 不超过40%/22%  | _/1.89G         |
|  qos2-p10k-3k-0         | _/31.37M             |    5      | 不超过23%/13%  | _/1.84G         |


qos2-p256-20K-0  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p256-20K-0.png)


qos2-p1k-10K-0  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p1k-10K-0.png)


qos2-p10k-3K-0  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p10k-3K-0.png)


--------------------
QoS2 Fan-Out消息吞吐
--------------------


|  组合场景ID              |  QoS  |  Payload  |  PUB连接 X Fan-In  |  SUB连接 X Fan-Out  | Fan-Out(平均/峰值)  |  背景连接   |
|-------------------------|-------|-----------|-------------------|--------------------|--------------------|------------|
|  qos2-p256-4-20K        |  2    |  256      |  4 X 1            |  5K X 4            |  14123             |  100K       |
|  qos2-p1K-2-10K         |  2    |  1K       |  2 X 1            |  5K X 2            |  7641              |  100K       |
|  qos2-p10K-1-1K         |  2    |  10K      |  1 X 1            |  1K X 1            |  935               |  100K       |


资源占用报告:


|  组合场景ID              | 网络吞吐(Avg/Max Bps) | 负载(Load) | CPU(user/sys) | Memory(Avg/Max) |
|-------------------------|----------------------|-----------|---------------|-----------------|
|  qos2-p256-4-20K        |  _/9.95M             |     8     | 不超过52%/30%  |   3.21G         |
|  qos2-p1k-2-10K         |  _/13.05M            |     7     | 不超过36%/26%  |   3.22G         |
|  qos2-p10k-1-1K         |  _/10.93M            |    3.2    | 不超过17%/14%  |   2.84G         |


qos2-p256-4-20K  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p256-4-20K.png)


qos2-p1k-2-10K  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p1k-2-10K.png)


qos2-p10k-1-1K  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p10k-1-1K.png)


--------------
共享订阅
--------------

订阅方式: $queue/<topic> 或 $share/<group>/<topic>


|  组合场景ID              |  QoS  |  Payload  |  PUB连接 X Fan-In  |  SUB连接 X Fan-Out  |  Fan-In (平均/峰值) |  Fan-Out(平均/峰值) |  背景连接   |
|-------------------------|-------|-----------|-------------------|---------------------|-------------------|--------------------|-----------|
|  qos0-p64-20K-20K       |  0    |  64       |  2K X 10          |  10 X 2K            |   18902           |    18891           |  100K     |
|  qos0-p256-20K-20K      |  0    |  256      |  2K X 10          |  10 X 2K            |   18874           |    18866           |  100K     |
|  qos1-p64-15K-15K       |  1    |  64       |  1.5K X 10        |  10 X 1.5K          |   13983           |    13939           |  100K     |
|  qos1-p256-15K-15K      |  1    |  256      |  1.5K X 10        |  10 X 1.5K          |   14002           |    13957           |  100K     |
|  qos2-p64-10K-10K       |  2    |  64       |  1K X 10          |  10 X 1K            |   8864            |    8860            |  100K     |
|  qos2-p256-7K-7K        |  2    |  256      | 0.7K X 10         |  10 X 0.7K          |   673             |    673             |  100K     |


资源占用报告:


|  组合场景ID         | 网络吞吐(RX / TX Bps) | 负载(Load) | CPU(user/sys) | Memory(Avg/Max) |
|--------------------|---------------------|------------|---------------|-----------------|
|  qos0-p64-20K-20K  |   4.84M/4.28M       |     8      | 不超过54%/26%  |   3.09G         |
|  qos0-p256-20K-20K |   8.52M/8.07M       |     7      | 不超过54%/26%  |   3.00G         |
|  qos1-p64-15K-15K  |   4.52M/3.80M       |     8      | 不超过56%/28%  |   3.05G         |
|  qos1-p256-15K-15K |   7.32M/6.61M       |     8      | 不超过58%/27%  |   3.07G         |
|  qos2-p64-10K-10K  |   4.68M/3.75M       |     8      | 不超过60%/30%  |   3.07G         |
|  qos2-p256-7K-7K   |    610k/477K        |     5      | 不超过20%/16%  |   2.78G         |


qos0-p64-20K-20K  EMQX服务器资源指标监控：

![image](./_static/images/qos0-p64-20K-20K.png)


qos0-p256-20K-20K  EMQX服务器资源指标监控：

![image](./_static/images/qos0-p256-20K-20K.png)


qos1-p64-15K-15K  EMQX服务器资源指标监控：

![image](./_static/images/qos1-p64-15K-15K.png)


qos1-p256-15K-15K  EMQX服务器资源指标监控：

![image](./_static/images/qos1-p256-15K-15K.png)


qos2-p64-10K-10K  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p64-10K-10K.png)


qos2-p256-7k-7K  EMQX服务器资源指标监控：

![image](./_static/images/qos2-p256-7K-7K.png)