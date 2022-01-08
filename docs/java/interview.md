# Java面试题



## Spring

### Spring MVC

#### Spring MVC的工作流程

Spring MVC处理模型数据的方式：

- 将方法返回值设置为`ModelAndView`

```java
@RequestMapping("/test")
public ModelAndView testModelAndView() {
    // 创建ModelAndView对象
    ModelAndView mav = new ModelAndView();
    // 设置模型数据，最终会放到request域中
    mav.putObject("user", "jho");
    // 设置视图
    mav.setViewName("success");
    return mav;
}
```

- 将方法返回值设置为`String`，在方法的入参种传入`Map`、`Model`、`ModelMap`。

```java
@RequestMapping("/test")
public String testMap(Map<String, Object> map) {
    // 向map中添加模型数据，最终会自动放到request域中
	map.put("user", new Employee(1, "jho", new Dept(1, "开发部")));
    return SUCCESS;
}
```

不管将处理器**方法的返回值**设置为`ModelAndView`还是在**方法的入参**中传入`Map`、`Model`、`ModelMap`，Spring MVC都会转换为一个`ModelAndView`对象。

Spring MVC的运行流程

```mermaid
sequenceDiagram
 	autonumber
	participant user as 用户
	participant dispatcher as DispatcherServlet<br />中央控制器
	participant hm as HandlerMapping<br />处理器映射器
	participant ha as HandlerAdapter<br />处理器设配器
	participant handler as Handler<br />(即Controller)
	participant vr as ViewResolver<br />视图解析器
	
	user->>dispatcher:发送请求
    dispatcher->>hm:调用处理器映射器获取处理器
    hm-->>dispatcher:返回HandlerExecutionChain<br />（HandlerIntercepter处理器拦截器+Handler处理器对象）
    dispatcher->>ha:通过处理器适配器调用具体处理器
    ha->>handler:调用处理器
    handler-->>handler:处理请求
    handler-->>ha:返回ModelAndView
    ha-->>dispatcher:返回ModelAndView
    dispatcher->>vr:调用视图解析器
    vr-->>vr:解析视图
    vr-->>dispatcher:返回view
    dispatcher->>dispatcher:调用view渲染视图
    dispatcher->>user:响应请求
```



## MyBatis