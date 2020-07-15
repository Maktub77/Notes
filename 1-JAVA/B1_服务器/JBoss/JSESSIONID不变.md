#### [jboss4.2GA版本下jsessionid不变的问题](https://gongmeitao.iteye.com/blog/2224565)

- 通过漏洞扫描工具来发现web网站存在的安全漏洞，修复这些漏洞来提高网站的安全系数。不过并不是所有的开发工程师都能解决所有的问题，因为没有人全能。

​      在tomcat5.5 tomcat6版本上，java 开发调用session的invalidate()方法可以修改sessionid，没有问题。不过在jboss4.2GA上就有问题了，因为../deploy/jboss-web.deployer/server.xm中emptySessionPath="true"，将true设置为false之后，重新启动jboss，在调用session的invalidate()方法，此时查看浏览器的jsessionid就会发生变化。