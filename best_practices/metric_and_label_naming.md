## 度量指标和标签命名
---
本文档中提供的指标和标签约定不需要使用Prometheus，但可以作为风格指南和最佳实践的集合。个别组织可能想要采取其中一些做法，例如：不同的命名约定。

### 度量指标名称
一个度量指标名称...
 - ... 应该有一个与单位所属的域相关的（单个单词）应用程序前缀。前缀有时成为客户端库的命名空间。对于特定于应用程序的度量指标，前缀通常是应用程序名称本身。然而，有时候，指标更为通用，如客户端导出的标准化指标。例子：

     - `prometheus_notifications_total`(指定Prometheus服务)
     - `process_cpu_seconds_total`(由许多客户库导出)
     - `http_request_duration_seconds`(所有HTTP请求)
- ...必须有一个单位（既不用毫秒或者秒与字节混合秒）.
- ...应该有一个基本单位（例如：秒，字节，米，不是毫秒，兆字节，公里）
- ...应该有一个描述单位的后缀，复数形式。请注意，除了适用的单位之外，积累计数总计作为后缀。
     - `http_request_request_seconds`
     - `node_memory_usage_bytes`
     - `http_requests_total`(无单位累计计数)
     - `process_cpu_seconds_total`(有单位的累计计数)
- ...应该代表在所有标签维度上测量的相同逻辑东西
     - 请求持续时间
     - 传输的字节数
     - 即时资源使用率百分比

作为经验法则，给定度量指标所有维度上的`sum()`或`avg()`应该是有意义的（尽管不一定有用）。如果没有意义，将数据分成多个指标。例如，在一个度量中具有各种队列的容量是好的，而将队列的容量与队列中的当前数量的元素混合是不好的。

### 标签
使用标签来区分正在测量的事物特征：
 - `api_http_requests_total`- 区分请求类型： `type="created| update | delete`.
 - `api_request_duration_seconds`- 区分请求阶段：`stage="extract | transform | load"` 

不要将标签名称放在度量指标名称下面，因为这会导致冗余，如果响应的标签被聚合，会导致混淆。

谨慎：请记住，键值标签对的每个独特组合都代表了一个新的时间序列，可以显着增加存储的数据量。 不要使用标签来存储具有高基数（许多不同标签值）的维度，例如用户ID，电子邮件地址或其他无限制的值集合。
