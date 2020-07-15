##### 拦截器获取Controller的返回值

```java
@ControllerAdvice
public class LoginLogAnalysis implements ResponseBodyAdvice {

    private static final String LOGIN_URI = "/login";

    @Override
    public boolean supports(MethodParameter returnType, Class converterType) {
        return true;
    }

    @Override
    public Object beforeBodyWrite(Object body, MethodParameter returnType, MediaType selectedContentType, Class selectedConverterType, ServerHttpRequest request, ServerHttpResponse response) {

        String requestPath = request.getURI().getPath();
		
       //获取指定访问接口的返回值
        if(requestPath.equals(LOGIN_URI)){
			//通过RequestContextHolder获取request
            HttpServletRequest httpServletRequest = 
                ((ServletRequestAttributes) RequestContextHolder                                                  .getRequestAttributes()).getRequest();

            HttpSession httpSession = httpServletRequest.getSession(true);
            httpSession.setAttribute("body", body);

            return body;
        }
        return body;
    }
}
```

```java
public class LoginInterceptor implements HandlerInterceptor {

    private static final Logger log = LoggerFactory.getLogger(LoginInterceptor.class);


    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object o) throws Exception {
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object o, Exception e) throws Exception {

        //获取Controller返回值
        Map logBodyMap = (Map) request.getSession().getAttribute("body");
        
        log.info("输出日志信息");

    }
}
```

