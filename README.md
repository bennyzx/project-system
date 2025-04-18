# 项目计划系统project-system

## 各领域对象组织原则
    在领域对象组织上，可以采用分包（Package）原则，从代码上明确聚合之间的边界，一个聚合即可以视为一个包，包中含有该聚合的所有实体对象和值对象。
    本项目涉及到的3个域，Core核心域、Discussion支撑域、UserCenter通用域对应的应用层、基础设施层的代码结构也应该严格遵循分包原则。

## 面向领域的项目组织方式
    参考平面形架构，面向领域的项目组织上一般从内而外，围绕核心领域模型展开。构建每一个子域的过程通常都是先抽象领域层再根据用户界面需求梳理应用服务，而各种资源库实现、外部服务集成都可以看成适配器。
    以Hibernate、MyBatis为代表的ORM（Object Relation Mapping，对象关系映射）框架可用于构建基于MySQL等关系型数据库的资源库。

## 上下文集成采用的实现技术
    我们综合使用统一协议（UP，Unified Protocol）和防腐层策略（ACL，Anticorruption Layer）并采用多种实现技术。
    一方面：UserCenter上下文使用基于HTTP的RESTful风格发布用户验证服务，而Discussion上下文则构建适配器风格的防腐层。
          这个层在两个模型之间（即Core和UserCenter）进行必要的双向转换。
          防腐层通常需要提供个性化实现，而统一协议的构建可以基于主流的Spring MVC、Jersey等框架。
    另一方面：Discussion和Core之间基于领域事件进行交互，RabbitMQ、ActiveMQ等主流消息中间件可以用于构建跨上下文之间的领域事件的发布和订阅机制。

## 采用测试驱动验证方式演进
    该项目的实现采用"测试驱动验证"的方式逐步演进，即采用提供基本实现、测试发现问题、重构解决问题的循环过程。
    通过引入JUnit、EasyMock等工具可以简单实现单元测试和集成测试。
    验证内容包括：实体和值对象、领域服务、领域事件、资源库接口、应用服务接入和上下文集成