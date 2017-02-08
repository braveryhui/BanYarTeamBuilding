# DevOps

## 什么是DevOps
DevOps是`Development`和`Operations`的组合。DevOps是一种开发、测试、运营、维护部门之间沟通、协作与整合的软件过程、方法与系统。

DevOps是一种高度强调人与人间互动的工作方式，不能先入为主地认为参与者了解某方面技能，在完成高频率部署的同时，提高生产环境的可靠稳定和安全行。

DevOps能够为团队提供一种极具凝聚力的文化氛围，DevOps不光是一个方法理念，而且是一个有力的技术手段，人员、文化、流程与工具这几大要素在DevOps中同样重要。

## 为什么用DevOps

与传统的`瀑布式开发`模型相比，采用敏捷或迭代式开发意味着更频繁的发布、每次发布包含的变化更少。由于部署经常进行，因此每次部署不会对生产系统造成巨大影响，应用程序会以平滑的速率逐渐生长。

> 瀑布模型是最典型的预见性的方法，严格遵循预先计划的需求、分析、设计、编码、测试的步骤顺序进行。
瀑布式的主要的问题是它的严格分级导致的自由度降低，项目早期即作出承诺导致对后期需求的变化难以调整，代价高昂。瀑布式方法在需求不明并且在项目进行过程中可能变化的情况下基本是不可行的。
大体分为这几个阶段：需求分析、设计、编码、测试、维护。
![瀑布模型](http://wiki.mbalib.com/w/images/d/df/%E7%80%91%E5%B8%83%E6%A8%A1%E5%9E%8B1.jpg)

一款产品的诞生不仅不能缺少开发人员，也离不开运维人员，开发和运维是不可分割的。而DevOps提供的方法恰好是把这两项工作密切结合在一起。

DevOps最吸引人的地方就是致力于把不同部门不同分工的人召集到一起，共同努力解决问题：

- 高效交付，这也正好是它的初衷；
- 会改善公司组织文化、提高员工的参与感。

## DevOps工具

### 常见工具

- 代码管理（SCM）：GitHub、GitLab、BitBucket、SubVersion
- 构建工具：Ant、Gradle、maven
- 自动部署：Capistrano、CodeDeploy
- 持续集成（CI）：Bamboo、Hudson、Jenkins
- 配置管理：Ansible、Chef、Puppet、SaltStack、Rock GuardRail
- 云/IaaS、PaaS：Amazon Web Services、Azure等
- 容器：Docker、LXC、第三方厂商如AWS
- 编排：Kubernetes、Core、Apache Mesos、DC/OS
- 服务注册与发现：Zookeeper、etcd、Consul
- 脚本语言：python、ruby、shell
- 日志管理：ELK、Logentries
- 系统监控：Datadog、Graphite、Icinga、Nagios
- 性能监控：AppDynamics、New Relic、Splunk
- 压力测试：JMeter、Blaze Meter、loader.io
- 自动化测试: 包括客户端与服务器端的自动化测试框架，例如Appium，Selenium 以及各种Mock技术和xUnit
- 预警：PagerDuty、pingdom、厂商自带如AWS SNS
- 安全：Snort、CyberArk等
- 消息总线：ActiveMQ、SQS
- 应用服务器：Tomcat、JBoss
- Web服务器：Apache、Nginx、IIS
- 数据库：MySQL、Oracle、PostgreSQL等关系型数据库；cassandra、mongoDB、redis等NoSQL数据库
- 项目管理（PM）、协作：Jira、Asana、Taiga、Trello、Basecamp、Pivotal Tracker、Worktile
- 质量、Bug反馈： Jira 是个不错的选择，其他的开源工具例如禅道，bugzila，mantis等等
- 文档管理：各种开发、运维、部署文档的统一管理


### 我们在用的

- 代码管理：Git
- 部署：Git
- 持续集成（CI）：Git
- 配置管理：Git
- 云/IaaS、PaaS：阿里云
- 容器：Docker，暂未投入生产环境
- 编排：暂无
- 服务注册与发现：暂无
- 脚本语言：python，但暂无应用；shell
- 日志管理：后台日志管理
- 系统监控：阿里
- 性能监控：阿里云
- 压力测试：手动、脚本
- 自动化测试: 暂无
- 预警：阿里云
- 安全：暂无
- 消息总线：暂无
- Web服务器：Nginx、Swoole
- 数据库：MySQL
- 项目管理（PM）、协作：Worktile
- 质量、Bug反馈： Worktile
- 文档管理：Git、Worktile


我们尚未完善的：

- 自动化测试：包括单元测试、自动化测试
- 压力测试
- 容器化：暂未投入生产环境
- 文档管理：未完善

我们所缺失的或者属于盲点的：

- 自动化部署
- 系统监控：目前依赖阿里云
- 性能监控：目前依赖阿里云
- 安全

