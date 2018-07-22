#### 微服务之 Spring Cloud
``` 
Spring Framework -> Spring Boot -> Spring Cloud

Spring Cloud Netflix：
  Eureka：服务注册与发现
  Zuul：路由与负载均衡
  Hystrix：服务熔断与监控
Spring Cloud Config：
  配置中心：把配置放到远程服务器，集中化管理集群配置，目前支持本地存储、Git以及Svn。
Spring Cloud Bus：
  事件、消息总线，用于在集群（例如，配置变化事件）中传播状态变化，可与Spring Cloud Config联合实现热部署。
Spring Cloud Sleuth：
  日志收集工具包，封装了Dapper和log-based追踪以及Zipkin和HTrace操作，
  为SpringCloud应用实现了一种分布式追踪解决方案。
Spring Cloud Stream：
  创建消息驱动微服务应用的框架。数据流操作开发包，封装了与Redis,Rabbit、Kafka等发送接收消息。
  
```

- 1.常见组件
``` 
Eureka：服务注册与发现
Feign：服务通信
Ribbon：客户端负载均衡器
Config：统一配置中心
Stream：数据流操作开发包
Zuul：路由和负载均衡器
Hystrix：服务熔断与监控
Sleuth：服务追踪
Zipkin：

```

- 2.组件简介：
``` 
1.Eureka：服务注册与发现  
  Eureka Server：提供服务注册和发现
  Service Provider：将自身服务注册到Eureka，从而使服务消费方能够找到
  Service Consumer：从Eureka获取注册服务列表，从而能够消费服务
2.Feign：服务通信
3.Ribbon：客户端负载均衡器
4.Config：统一配置中心
5.Stream：数据流操作开发包
6.Zuul：网关服务，路由和负载均衡器
7.Hystrix：服务熔断与监控
8.Sleuth：服务追踪
  一个分布式服务跟踪系统，主要有三部分：数据收集、数据存储和数据展示。
  为服务之间调用提供链路追踪。
  sleuth可以结合zipkin，将信息发送到zipkin，
  利用zipkin的存储来存储信息，利用zipkin ui来展示数据
9.Zipkin：
  开放源代码分布式的跟踪系统，由Twitter公司开源
  Zipkin提供了可插拔数据存储方式：In-Memory、MySql、Cassandra以及Elasticsearch。
  接下来的测试为方便直接采用In-Memory方式进行存储，生产推荐Elasticsearch。
```