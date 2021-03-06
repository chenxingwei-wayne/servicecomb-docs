#Http2

## 场景描述

用户通过简单的配置即可启用Http2进行通信，提高性能。

## 外部服务通信配置

与外部服务通信相关的配置写在microservice.yaml文件中。

* 启用h2\(Http2 + TLS\)进行通信  
  服务端在配置服务监听地址时，可以通过在地址后面追加`sslEnabled=true`开启TLS通信，具体介绍见[使用TLS通信](../../security/tls.md)章节。然后再追加`protocol=http2`启用h2通信。示例如下：

  ```yaml
  servicecomb:
    rest:
      address: 0.0.0.0:8080?sslEnabled=true&protocol=http2
  ```

* 启用h2c\(Http2 without TLS\)进行通信  
  服务端在配置服务监听地址时，可以通过在地址后面追加`protocol=http2`启用h2c通信。示例如下：

  ```yaml
  servicecomb:
    rest:
      address: 0.0.0.0:8080?protocol=http2
  ```

* 客户端会通过从服务中心读取服务端地址中的配置来使用http2进行通信。 

具体实例可以参考 [http2-it-tests](https://github.com/apache/servicecomb-java-chassis/blob/master/integration-tests/it-consumer/src/main/java/org/apache/servicecomb/it/ConsumerMain.java)

## http2 server 端配置项

| 配置项                                        | 默认值  | 含义                                    | 注意 | 
|-----------------------------------------------|---------|---------------------------------------- |------|
|servicecomb.rest.server.http2.useAlpnEnabled   | true    |是否启用 ALPN                            |      |
|servicecomb.rest.server.http2.concurrentStreams| 100     |一条连接中，同时支持的最大的stream并发量 |以server端的concurrentStreams和client端的multiplexingLimit较小值为准|

## http2 client 端配置项

| 配置项                                            | 默认值 | 含义                                                  | 注意 | 
|---------------------------------------------------|--------|------------------------------------------------------ |------|
|servicecomb.rest.client.http2.useAlpnEnabled       |true    |是否启用 ALPN                                          |      |
|servicecomb.rest.client.http2.multiplexingLimit    |-1      |一条连接中，同时支持的最大的stream并发量，-1表示不限制 |以server端的concurrentStreams和client端的multiplexingLimit较小值为准 |
|servicecomb.rest.client.http2.maxPoolSize          |1       |每个连接池中，对每一个IP：Port最多建立的连接数         |      |
|servicecomb.rest.client.http2.idleTimeoutInSeconds |0       |空闲连接的超时时间，超时后会关闭连接                   |      |

