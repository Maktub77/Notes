#### Struts2 拦截器中invocation.invoke()

```java
public String intercept(ActionInvocation actionInvocation) {
    //取得session
	Map<?, ?> session = actionInvocation.getInvocationContext().getSession();
    HttpServletResponse response = ServletActionContext.getResponse();
    HttpServletRequest request = ServletActionContext.getRequest();
    //从Session里获得登录时保存进session的User类
    String user = (String) session.get(USER_SESSION_KEY);
    //判断用户名是否为空  
    if (null!=user) {
        return Action.LOGIN;   //返回登录页面  
    }else{       
        // 下面的这个actionInvocation.invoke()是什么意思
        return actionInvocation.invoke();//返回验证通过         
    }  
}
```

`invocation.invoke()`就是通知struts2接着干下面的事情

比如：调用下一个拦截器或者执行下一个Action

就相当于退出了当前编写的这个interceptor

