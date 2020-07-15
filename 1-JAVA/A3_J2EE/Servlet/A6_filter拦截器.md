### Filter

* 拦截器：主要用于对用户请求进行预处理，也可以对`HttpServletResponse`进行后处理，是个典型的处理链。
* 可以修改request中的信息（）

1. 创建Filter类
   * 创建Filter处理类
     * 创建一个类，实现Filter接口
     * 实现Filter接口中的抽象方法
   * web.xml文件中配置Filter

		过滤器（Filter）：对项目的资源发起请求的时候，可以通过配		  置的过滤器规则，对指定的资源请求进行过滤；执行完过滤中的过滤逻辑，再根据过滤器的放行规则，再判断决定是否对请求的资源放行。

* 过滤器的实现原理：基于Java的设计模式---代理模式-面向切面编程

* 执行过程

  1. 创建：当服务器启动时，会自动创建，调用构造方法和init方法

  2. 当客户端请求一个资源时，也会被所有设置过的拦截器进行拦截

     拦截顺序：按照web.xml中的<filter-name>标签中配置的字母字典顺序进行拦截

* 作用

  * 可以验证用户是否登录

    ```java
    public class LoginFilter implements Filter {
    
        private String [] go = {"login.jsp","reg.jsp","index.jsp",".js",".css",".png",".jpg"};
        public void destroy() {
    
        }
    
        public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
            HttpServletRequest request = (HttpServletRequest)req;
            HttpServletResponse response = (HttpServletResponse)resp;
            HttpSession session = request.getSession();
    
            //1.获取拦截的路径
            String path = request.getRequestURI();
    
            boolean isOK = false;
            for(String s : go){
                if(path.endsWith(s) || session.getAttribute("user") != null){
                    isOK = true;
                    break;
                }	
            }
    
            if(isOK){
                chain.doFilter(request, response);		
            }else{
                response.sendRedirect("login.jsp");
            }
    
        }
    
        public void init(FilterConfig fConfig) throws ServletException {
    
        }
    }
    ```

  * 日志记录

  * 统一解决乱码问题

    ```java
    public class EncodingFilter implements Filter {
    
        private String encode;
    
        public void init(FilterConfig fConfig) throws ServletException {
            encode = fConfig.getInitParameter("encode");
        }
    
        public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
            HttpServletRequest request = (HttpServletRequest)req;
            HttpServletResponse response = (HttpServletResponse)resp;
    
            request.setCharacterEncoding(encode);
            response.setCharacterEncoding(encode);
    
            chain.doFilter(request, response);
        }
    
        public void destroy() {
    
        }
    }
    ```

    ```xml
    <filter>
        <display-name>EncodingFilter</display-name>
        <filter-name>EncodingFilter</filter-name>
        <filter-class>com.filter.EncodingFilter</filter-class>
        <init-param>
            <param-name>encode</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>EncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ```

    