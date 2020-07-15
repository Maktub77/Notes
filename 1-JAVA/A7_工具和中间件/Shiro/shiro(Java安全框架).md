### [Shiro](http://how2j.cn/k/shiro/shiro-plan/1732.html)

* Subject 在 Shiro 这个安全框架下， Subject 就是当前用户
* Realm：当应用程序向 Shiro 提供了 账号和密码之后， Shiro 就会问 Realm 这个账号密码是否对， 如果对的话，其所对应的用户拥有哪些角色，哪些权限。 
  所以Realm 是什么？ 其实就是个中介。 Realm 得到了 Shiro 给的用户和密码后，有可能去找 ini 文件
* DatabaseRealm：用来通过数据库 验证用户，和相关授权的类。
  两个方法分别做验证和授权：
  doGetAuthenticationInfo(), doGetAuthorizationInfo()

