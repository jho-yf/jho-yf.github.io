# 开发规范



## 


### PO(Persistant Object)持久对象

PO就是对应数据库中某个表的一条记录，多个记录可以用PO的集合，PO中应该不包含任何对数据库的操作。



### DO(Domain Object) 领域对象

DO就是从现实世界中抽象出来的有形或无形的业务实体。



### TO(Transfer Object) 数据传输对象

不同的应用之间传输的对象



### DTO(Data Transfer Object) 数据传输对象

用于展示层与服务层之间的数据传输对象



### VO(Value Object) 视图对象

- 接受页面传递来的数据，封装对象
- 将业务处理完成的对象，封装成页面要用的数据



### BO(Business Object) 业务对象

Business Object业务对象，主要作用是把业务逻辑封装为一个对象。这个对象可以包括一个或多个其他的对象。比如：一个简历，有教育经历、工作经历、社会关系等。其中教育经历对应一个PO、工作经历对应一个PO、社会关系对应一个PO，建立一个对应简历的BO对象，每个BO包含这些PO。这样处理业务逻辑时，我们就可以针对BO去处理



### POJO(Plain Ordinary Java Object) 简单无规则Java对象

最基本的Java Bean，只有属性字段以及`setter`和`getter`方法。

POJO是DO、DTO、BO、VO的统称



### DAO(Data Access Object) 数据访问对象

Data Access Object数据访问对象，此对象用于访问数据库，负责持久层的操作，包含各种数据库的操作方法，为业务层提供接口。通常和PO结合对数据库进行相关操作，夹在业务逻辑与数据库资源中间，配合VO提供数据库的CRUD操作。

