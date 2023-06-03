# Quartz

> 官网： http://www.quartz-scheduler.org/documentation/

## 核心概念

任务`Job`：`Job`就是你想要实现的任务类，每个`Job`必须实现`org.quartz.job`接口，且只需实现接口定义的`execute()`方法

触发器`Trigger`：`Trigger`是执行任务的触发器

调度器`Scheduler`：`Scheduler`是任务的调度器，他会将`Job`以及触发器`Trigger`整合起来，负责基于`Trigger`设定的时间来执行`Job`