注册中心双备先启动一台的时候报错com.netflix.discovery.shared.transport.TransportException: Cannot execute request on any known server(独立部署).因为交叉配置,不能同时启动可能会出现上述错误,怎么来关闭这个呢?

ribbon(工具,和服务在同一个进程中):获取注册中心列表进行负载均衡,检查服务列表中的服务是否正常.原理,拦截器对resttemplate请求拦截,利用负载均衡器将服务名转化成具体的实例地址.

服务客户端:发现服务,注册服务,服务心跳,服务下线,服务列表缓存

服务注册中心:管理服务列表,定时清理下线服务.

服务异常下线之后并不会马上被服务注册中心检测到.因为服务心跳时间是30s,90s没有心跳发送才认为服务下线.

服务怎么下线?

region和zone是什么,两者关系是怎样的,他们主要有什么作用
Eureka还支持Region和Zone的概念。其中一个Region可以包含多个Zone。Eureka在启动时需要指定一个Zone名，即当前Eureka属于哪个zone, 如果不指定则属于defaultZone。Eureka Client也需要指定Zone, Client(当与Ribbon配置使用时)在向Server获取注册列表时会优先向自己Zone的Eureka发请求，如果自己Zone中的Eureka全挂了才会尝试向其它Zone。Region和Zone可以对应于现实中的大区和机房，如在华北地区有10个机房，在华南地区有20个机房，那么分别为Eureka指定合理的Region和Zone能有效避免跨机房调用，同时一个地区的Eureka坏掉不会导致整个该地区的服务都不可用。
指定服务zone:eureka.instance.metadata-map.zone=shanghai
怎么指定region呢?

服务注册中心核心配置:服务名称,端口,是否注册自己,是否获取服务,hostname,热备注册
服务客户端:实例id,端口号,服务名称,注册中心地址,所在zone

静态类的作用?
界面管理:注册中心,服务监控(熔断器),日志平台

怎么配置负载策略?

Hystrix:短路器,服务降级,服务熔断,线程和信号隔离,请求缓存,请求合并以及服务监控.
hystrix把请求封装成命令模式执行,每个命令都有key值(非必须)和分组(必须).主要依靠分组来统计命令的告警.仪表盘等信息.同时也根据组名来确定线程池.
怎么查看调用的服务是缓存结果还是实际结果呢?
请求缓存没有测试成功,

RxJava的使用,了解观察者模式,熟悉他的使用即可
java.lang.IllegalStateException: Request caching is not available. Maybe you need to initialize the HystrixRequestContext?

UI界面:注册中心,hystrix dashboard,

hystrix dashboard和任何工程无关,只是访问应用的接口做可视化展示而已

turbine需要收集应用的接口信息汇总后交给hystrix dashboard展示


zuul网关的负载均衡以及熔断机制
微服务的命名规则

对外服务80开头,内部服务90开头,配套服务11开头


ELK安装
