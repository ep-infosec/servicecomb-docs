# Saga
Apache ServiceComb Saga 是一个微服务应用的数据最终一致性解决方案。

## 特性
* 高可用。支持集群模式。
* 高可靠。所有的事务事件都持久存储在数据库中。
* 高性能。事务事件是通过gRPC来上报的，且事务的请求信息是通过Kyro进行序列化和反序列化的。
* 低侵入。仅需2-3个注解和编写对应的补偿方法即可进行分布式事务。
* 部署简单。可通过Docker快速部署。
* 支持前向恢复（重试）及后向恢复（补偿）。
* 扩展简单。基于Pack架构很容实现多种协调机制。

## 架构
Saga Pack 架构是由 **alpha** 和 **omega**组成，其中：
* alpha充当协调者的角色，主要负责对事务进行管理和协调。
* omega是微服务中内嵌的一个agent，负责对网络请求进行拦截并向alpha上报事务事件。

下图展示了alpha, omega以及微服务三者的关系：
![Saga Pack 架构](static_files/pack.png)
在此架构基础中我们除了实现saga协调协议以外，我们还可以很容易实现TCC协调协议。
详情可浏览[Saga Pack 设计文档](design_zh.md).

同时社区也提供了多种语言的Omega实现:
* Go语言版本Omega 可参见 https://github.com/jeremyxu2010/matrix-saga-go
* C#语言版本Omega 可参见 https://github.com/OpenSagas-csharp/servicecomb-saga-csharp


## 快速入门
* Saga在ServiceComb Java Chassis应用可以参考[出行预订](https://github.com/apache/servicecomb-pack/blob/master/demo/saga-servicecomb-demo/README.md)
* Saga在Spring应用的用法可参考[出行预订示例](https://github.com/apache/servicecomb-pack/blob/master/demo/saga-spring-demo/README.md)。
* Saga在Dubbo应用的用法可参考[Dubbo示例](https://github.com/apache/servicecomb-pack/blob/master/demo/saga-dubbo-demo/README.md).
* TCC在Spring应用的用法可以参考[TCC示例](https://github.com/apache/servicecomb-pack/blob/master/demo/tcc-spring-demo/README.md)
* 示例的的调试方法可以参考[调试Spring示例](https://github.com/apache/servicecomb-pack/blob/master/demo/saga-spring-demo#debugging).

## 编译和运行代码
* 编译代码并且运行相关的单元测试
   ```bash
      $ mvn clean install
   ```
* 编译示例，并生产docker镜像，运行验收测试
   ```bash
      $ mvn clean install -Pdemo,docker
   ```
* 当前Saga模块同时支持Spring Boot 1.x 以及 Spring Boot 2.x, 在缺省情况下Saga会使用Spring Boot 1.x来进行构建。
你可以使用 *-Pspring-boot-2* 将Spring Boot版本转换到 2.x 上。 由于Spring Boot 只在2.x开始支持 JDK9，如果你想用
JDK9或者JDK10 来编译Saga并运行测试的话，你需要使用 spring-boot-2 profile参数。
   ```bash
      $ mvn clean install -Pdemo,docker,spring-boot-2
   ```
