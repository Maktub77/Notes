吞吐量：是指对网络、设备、端口、虚电路或其他设施，单位时间内成功地传送数据的数量(以比特、字节、分组等测量)。

---

进程：表示资源分配的基本单位

线程：进程中执行运算的最小单位，即执行处理机调度的基本单位

一个程序有一个进程，而一个进程可以有多个线程

---

并发：在一段时间内同时做多个事情

​	在单核处理中同时处理多个任务（逻辑上的同时）

并行：在同一时刻做多个事情

​	在多核处理器中同时处理多个任务（物理上的同时）

---

springBoot的缓存机制是通过切面编程aop来实现的。执行缓存时要通过绕一绕的方式故意诱发aop，这样才会走redis缓存。

```java
@Component
public class SpringContextUtil implements ApplicationContextAware {  
    private SpringContextUtil() {

    }  
    private static ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext){
        SpringContextUtil.applicationContext = applicationContext;
    }

    public static <T> T getBean(Class<T> clazz) {
        return applicationContext.getBean(clazz);
    }
}
```

---

* JSP需要访问的时候访问Dao层抓取数据，生成html页面，返回给浏览器
* freeMarker则是提前根据模板生成好html的静态页面，这样在访问时，直接访问到的就是一个静态页面，这个就是效率问题了，不过freeMarker适合用变化不大，但是内容很多的网页。

---

