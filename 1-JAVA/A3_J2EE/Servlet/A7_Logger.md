日志记录过滤器

* logger.debug(Object message) 调试日志
* logger.info(Object message) 消息日志
* logger.warn(Object message) 警告日志
* logger.error(Object message) 错误日志
* logger.fatal(Object message) 内存不足日志

1. 创建日志类的核心对象Logger

   ```java
   private Logger log = Logger.getLogger(LoginFilter.class);
   ```

   ​	