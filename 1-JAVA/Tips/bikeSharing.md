用户登录，要解决的问题：

session一致性问题  采用session外置方式 采用redis集中管理

不是浏览器 没有cokies session怎么搞？

浏览器 session接口 有状态的 statefull

移动端 往往 都是无状态的nostatus 不创建session

session 能标识唯一会话 唯一用户的 就是session

* 用户登录的时候 生成一个token(相当于session) token存redis 设置超时 
* 用户每次请求都带着token 服务端校验

---

安全性问题

token代表唯一用户 如何保密

登录获取token的时候 传输要加密

md5 数字签名 不可逆  不能用 因为没法反解密

加密

* 对称加密：明文+key(秘钥) = 密文    解密：key+密文=明文
  * 加密 解密用的key是一样的 所以叫对称加密
  * 缺点：安全性依赖于key
  * 优点：效率比非对称加密高
* 非对称加密：加密 用的key 是不一样的 所以叫非对称加密
  * 公钥 私钥
  * 明文 + 公钥 = 密文    解密： 私钥 + 密文 = 明文  安全性高效率低
* 结合起来，使用对称加密加密数据，使用非对称加密加密key(秘钥)   先解密key 再使用key解密数据

说白了都是字符串

对称加密：AES DES 等等

非对称加密：RSA 典型 大因数分解

AES+RSA加密

HTTP 不安全 传输过程 完全暴露

HTTPS 证书

>加密要注意编码

---

Shiro   SpringSecurity

预授权：在别的系统 或者别的代码中 已经进行过授权、

授权：在本系统内部加载权限信息

pre预处理 manager------provider(一个或者N个 提供权限信息) entrypoint 统一异常处理

csrf 跨站脚本攻击 相当于伪造表单 