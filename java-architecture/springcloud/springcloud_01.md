#### 微服务之 Spring Cloud
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

1.微服务介绍
2.服务注册与发现 Eureka(找到了)
  Eureka Server
  Eureka Client
  Eureka的负载均衡是客户端实现
3.服务拆分
4.服务通信 RestTemplate & Feign
  Http vs RPC
      RPC -> Dubbo
      Http Restful -> Spring Cloud
  客户端负载均衡器：Ribbon
    RestTemplate & Feign 和网管服务Zuul都使用到了 Ribbon
5.统一配置中心 Config
  Config Server
  Config Client
  Bus 自动更新配置 集成github的WebHooks动态更新配置
6.消息与异步 Stream
  RabbitMQ
  Stream集成了消息队列服务
7.服务网关 Gateway & Zuul
  7*24小时可用
  Gateway方法：
    1).Nginx + Lua 性能极高 用户流量路由
    2).Kong 商业 得花钱
    3).Tyk Go语言开发
    4).Spring Cloud Zuul：路由和负载均衡器 Netflix 服务间路由
  路由+过滤器=Zuul：核心是一系列的过滤器
8.Zuul综合使用
9.服务容错 Hystrix
  Hystrix-Dashboard 控制台监控面板
10.服务追踪 
  Sleuth
  Zipkin
11.容器部署
  Docker + Rancher
```