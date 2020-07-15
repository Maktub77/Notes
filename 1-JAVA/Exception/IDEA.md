###### Intellij IDEA运行报Command line is too long解法

* 报错内容

  ```java
  Error running 'ServiceStarter': Command line is too long. Shorten command line for ServiceStarter or also for Application default configuration.
  ```

* 修改

  ```
  项目下 .idea\workspace.xml，
  找到标签 <component name="PropertiesComponent"> ， 
  在标签里加一行  <property name="dynamic.classpath" value="true" />
  ```


