
# 许集泓 - Java开发工程师 <br />
::: left
icon:info 男 | 1996.10 &emsp;&emsp; 籍贯：广东汕头 &emsp;&emsp; 现居地：广东广州
icon:school 广东技术师范大学&ensp; | &ensp;计算机科学与技术&ensp; | &ensp;2020年毕业 &ensp;| &ensp;全日制本科
icon:phone 联系方式：16620478627&emsp;&emsp;电子邮箱：xujihong.yif@gmail.com
icon:blog 英语水平：CET-4&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;相关资格证书：中级软件设计师

:::

::: right
<img src="https://s2.loli.net/2022/01/04/Xs3KdMP1HuVGi9Q.jpg" style="width:95px;position:absolute;top: 74px;right: 50px;">
:::

## 求职意向

&emsp;&emsp;&emsp;&emsp;**求职岗位：** Java开发工程师 &emsp; **|** &emsp; **期望薪资：** 17K-20K &emsp; **|** &emsp; **意向城市：** 广东广州

## 工作经历

### 省事熊智能科技有限公司 （2019.12-至今）
`Java开发` `Kubernetes运维开发` `RPA机器人流程自动化`
主要担任公司产品开发部**后台服务端核心开发**，兼**DevOps运维开发**。作为公司自研核心产品IBRPA 5.0的主力程序员，不但参与公司产品需求分析、系统设计，负责**后端基本框架的搭建**以及各个**微服务的开发**，还完成了产品的**Kubernetes服务集群**和**Docker私有化部署方案**的搭建。在保证后端本职工作完成的前提下，学习Python协助客户端同事完成Python相关服务和组件库的开发。
**工作期间主要成绩如下：** 
- 为产品的设计和研发提供了5个以上设计方案，解决10项以上技术难点，包括：流程队列实现方案、流程异常处理方案、流程变量流转方案、流程计划作业服务设计、流程运行速度缓慢问题、流程业务指令方案、服务私有化部署方案、权限控制方案、SaaS平台对接方案等。
- 为多家代理商（总计100+机器人流程自动化场景）提供了高效、便捷的IBRPA服务私有化部署方案，每个场景能够节省5个工作日左右的时间，极大的提高了实施工作效率。
- 为提高开发部、实施部工作效率，搭建各类服务、工具和CI/CD方案，包括：Harbor（Docker镜像存储和分发仓库）、devpi（Python包发布、上传、测试系统）、Sentry（实时事件日志监控、记录和聚合平台）、云效流水线等。
- 曾获公司2020年度最佳新人奖、2020年度最佳团队奖。


## 主要项目经历

### 省事熊IBRPA 5.0 | 核心开发

`Spring Cloud` `Activiti` `XXL-JOB` `Kubernetes` `MQTT` `gRPC`
省事熊IBRPA 5.0是基于Petri网的智能**RPA引擎**。后台基于Spring Cloud + Spring Boot搭建微服务基本模块，使用Activiti作为流程引擎，使用Spring Cloud Stream集成RibbitMQ实现服务间的异步通信，使用XXL-JOB分布式任务调度平台做流程定时作业的调度，使用基于MQTT协议的消息服务器EMQX下发指令来调度客户端运行，使用K8S+Docker+云效流水线作为CI/CD方案。

**负责模块：** 工作流引擎运行时服务、工作流引擎连接器、流程运行历史查询服务、流程指挥中心（流程任务指令翻译下发、流程队列编排）、流程计划作业调度服务、统一认证中心、模板仓库服务

---
**(未完请转下页)**

<br />

**主要技术点：**
- 使用Redis作为**Activiti流程定义缓存**，避免服务重启或者宕机导致的缓存数据丢失的现象，解决了当流程定义过多时导致的内存溢出和流程运行缓慢等问题，将原本单个任务运行时间由秒级别降低至毫秒级别。
- 使用**Spring Cloud Kubernetes**将Spring Cloud服务构建和运行在K8S上。将K8S的**Service**作为注册中心，实现服务发现和熔断。使用K8S的**ConfigMap**作为配置中心，实现配置的动态刷新。
- 为避免用户多次启动流程，导致当前流程未结束运行，就开始运行下一个流程任务的情况。引入**流程队列**的方案，使用**Redis Stream消息队列**，最终实现同一终端多个流程排队执行，每个终端同一时刻只运行一个流程，其他流程排队依次执行。

产品下载链接：https://www.ibrpa.com/products/studio

### 宁德核电机电自动化管理平台 | 项目经理 + 核心开发
`Spring Boot` `Redis` `Docker` `Robot Framework`
宁德核电机电自动化管理平台是宁德核电机电部内部的自动化管理平台，主要包括：邮件任务、日常、大修、安全、备件、培训、经验反馈等模块。该平台主要结合RPA技术，实现机电部**邮件任务自动跟踪统计**工作和各模块数据的自动化处理工作，包括数据导出、清洗、处理、统计、展示等。
**主要职责：** 作为项目经理和后端开发，从前期的需求调研、需求分析、总体设计方案，到开发过程中的数据库设计、邮件模块的代码编写、集成测试，到最后的实施部署，都完全参与其中。
**主要技术点：**
为完成邮件的自动跟踪的功能，使用Robot Freamwork定时读取Outlook邮件，获取邮件标题、邮件内容、邮件附件等信息，使用正则匹配邮件内容中的各个关键字，之后提交到后台服务进行存库以便后续的对于邮件任务的统计分析。



## 技术清单

以下均为我熟练使用的技能

【开发语言】&emsp;`Java`&ensp;`Python`&ensp;`Shell`


【框架】&emsp;&emsp;&emsp;`Spring`&ensp;`Spring Boot`&ensp;`Activiti`&ensp;`Mybatis`&ensp;`Django`

【微服务】&emsp;&emsp;`Spring Cloud`&ensp;`Spring Cloud Alibaba`&ensp;`gRPC`&ensp;`Nameko`

【中间件】&emsp;&emsp;`MySQL`&ensp;`PostgreSQL`&ensp;`Redis`&ensp;`RabbitMQ`&ensp;`EMQ X`

【工具】&emsp;&emsp;&emsp;`Maven`&ensp;`Git`&ensp;`IntelliJ IDEA`&ensp;`VS Code`&ensp;`Postman`

【部署】&emsp;&emsp;&emsp;`Linux`&ensp;`Vagrant`&ensp;`Docker`&ensp;`Docker Compose`&ensp;`Kubernetes`&ensp;`Helm`

## 自我评价
- 现任职公司在政务、医疗、银行、能源、教育等行业均有所涉及，具备相关业务能力。
- 产品开发需要时，能够快速学习相应技术框架协助同事推进工作。
- 能够通过阅读源代码或借助技术文档、开源技术社区、搜索引擎等网络资源寻找技术方案，解决技术难点。
- 项目实施需要时，参与一线需求调研，与客户现场、技术团队以及销售团队多方协调，多次协调并带领实施团队完成项目验收。



---
**（感谢您花时间阅读我的简历，期待能有机会和您共事。）**