# JWT

https://blog.csdn.net/achenyuan/article/details/80829401

> redis保存session，使用shiro做登陆

* json web token

* 将用户信息加密到token里，服务器不保存任何用户信息。服务器通过使用保存的密匙验证token的正确性，只要正确即通过验证。

* 优点：在分布式系统中，很好地解决了单点登录问题，很容易解决了session共享的问题。

  缺点：无法作废已颁布的令牌/不易应对数据过期。

## JWT基本使用

* 在pom.xml引入java-jwt

  ```xml
  <dependency>
      <groupId>com.auth0</groupId>
      <artifactId>java-jwt</artifactId>
      <version>3.4.0</version>
  </dependency>
  ```

* Gradle下依赖

  ```groovy
  compile 'com.auth0:java-jwt:3.4.0'
  ```

* 实例

  ```java
  package yui.ui.web.sys.controller;
  
  import java.util.Calendar;
  import java.util.Date;
  import java.util.HashMap;
  import java.util.Map;
  
  import com.alibaba.druid.util.StringUtils;
  import com.auth0.jwt.JWT;
  import com.auth0.jwt.JWTVerifier;
  import com.auth0.jwt.algorithms.Algorithm;
  import com.auth0.jwt.interfaces.Claim;
  import com.auth0.jwt.interfaces.DecodedJWT;
  
  public class Test {
      /**
       * APP登录Token的生成和解析
       * 
       */
  
      /** token秘钥，请勿泄露，请勿随便修改 backups:JKKLJOoasdlfj */
      public static final String SECRET = "JKKLJOoasdlfj";
      /** token 过期时间: 10天 */
      public static final int calendarField = Calendar.DATE;
      public static final int calendarInterval = 10;
  
      /**
       * JWT生成Token.<br/>
       * 
       * JWT构成: header, payload, signature
       * 
       * @param user_id
       *            登录成功后用户user_id, 参数user_id不可传空
       */
      public static String createToken(Long user_id) throws Exception {
          Date iatDate = new Date();
          // expire time
          Calendar nowTime = Calendar.getInstance();
          nowTime.add(calendarField, calendarInterval);
          Date expiresDate = nowTime.getTime();
  
          // header Map
          Map<String, Object> map = new HashMap<>();
          map.put("alg", "HS256");
          map.put("typ", "JWT");
  
          // build token
          // param backups {iss:Service, aud:APP}
          String token = JWT.create().withHeader(map) // header
              .withClaim("iss", "Service") // payload
              .withClaim("aud", "APP").withClaim("user_id", null == user_id ? null : user_id.toString())
              .withIssuedAt(iatDate) // sign time
              .withExpiresAt(expiresDate) // expire time
              .sign(Algorithm.HMAC256(SECRET)); // signature
  
          return token;
      }
  
      /**
       * 解密Token
       * 
       * @param token
       * @return
       * @throws Exception
       */
      public static Map<String, Claim> verifyToken(String token) {
          DecodedJWT jwt = null;
          try {
              JWTVerifier verifier = JWT.require(Algorithm.HMAC256(SECRET)).build();
              jwt = verifier.verify(token);
          } catch (Exception e) {
              // e.printStackTrace();
              // token 校验失败, 抛出Token验证非法异常
          }
          return jwt.getClaims();
      }
  
      /**
       * 根据Token获取user_id
       * 
       * @param token
       * @return user_id
       */
      public static Long getAppUID(String token) {
          Map<String, Claim> claims = verifyToken(token);
          Claim user_id_claim = claims.get("user_id");
          if (null == user_id_claim || StringUtils.isEmpty(user_id_claim.asString())) {
              // token 校验失败, 抛出Token验证非法异常
          }
          return Long.valueOf(user_id_claim.asString());
      }
  }
  ```

  最终存放的数据在JWT内部的实体claims里。它是存放数据的地方

## 概念介绍

### JWT消息构成

一个token分3部分(由三部分生成token  3部分之间用“`.`”号做分隔)，按顺序为：

* 头部（header)：承载两部分信息

  * 声明类型，这里是jwt

  * 声明加密的算法 通常直接使用 HMAC SHA256 
     JWT里验证和签名使用的算法，可选择下面的。

  * 使用的代码如下：

    ```java
    // header Map
    Map<String, Object> map = new HashMap<>();
    map.put("alg", "HS256");
    map.put("typ", "JWT");
    ```

* 载荷（payload)

  * 载荷就是存放有效信息的地方。基本上填2种类型数据

    -标准中注册的声明的数据 
     -自定义数据 
     由这2部分内部做base64加密。最张数据进入JWT的chaims里存放。

  * **标准中注册的声明（建议但不强制使用）**

    ```java
    iss: jwt签发者
    sub: jwt所面向的用户
    aud: 接收jwt的一方
    exp: jwt的过期时间，这个过期时间必须要大于签发时间
    nbf: 定义在什么时间之前，该jwt都是不可用的.
    iat: jwt的签发时间
    jti: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。
    ```

  * 使用方法

    ```java
    JWT.create().withHeader(map) // header
        .withClaim("iss", "Service") // payload
        .withClaim("aud", "APP")
        .withIssuedAt(iatDate) // sign time
        .withExpiresAt(expiresDate) // expire time
    ```

  * **自定义数据**

    这个就比较简单，存放我们想放在token中存放的key-value值 

    使用方法

    ```java
    JWT.create().withHeader(map) // header
        .withClaim("name", "cy") // payload
        .withClaim("user_id", "11222");
    ```

* 签证（signature) 

  jwt的第三部分是一个签证信息，这个签证信息算法如下： 
  `base64UrlEncode(header) + "." +  base64UrlEncode(payload)+your-256-bit-secret `

  这个部分需要base64加密后的header和base64加密后的payload使用.连接组成的字符串，然后通过header中声明的加密方式进行加盐secret组合加密，然后就构成了jwt的第三部分。

## JJWT

它是为了更友好在JVM上使用JWT,是基于JWT，JWS，JWE，JWK框架的java实现

### 引入

Maven

```xml
<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt</artifactId>
<version>0.9.0</version>
</dependency>
```

Gradle

```java
dependencies {
    compile 'io.jsonwebtoken:jjwt:0.9.0'
}
```

### 使用方法

#### 生成token

getJwtToken是生成jjwt里的token方法

```java
import com.sun.javafx.scene.traversal.Algorithm;
import io.jsonwebtoken.*;
import io.jsonwebtoken.impl.DefaultJwsHeader;

import java.util.Calendar;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;


private String  SECRET = "DyoonSecret_0581";
private void getJwtToken(){
    Date iatDate = new Date();
    // expire time
    Calendar nowTime = Calendar.getInstance();
    //有10天有效期
    nowTime.add(Calendar.DATE, 10);
    Date expiresDate = nowTime.getTime();
    Claims claims = Jwts.claims();
    claims.put("name","cy");
    claims.put("userId", "222");
    claims.setAudience("cy");
    claims.setIssuer("cy");
    String token = Jwts.builder().setClaims(claims).setExpiration(expiresDate)
        .signWith(SignatureAlgorithm.HS512, SECRET)
        .compact();
}
```

上面将token中的荷载放在chaims中，其实chaims是JWT内部维持的一个存放有效信息的地方，不论使用任何API，最终都使用chaims保存信息

`setClaims`有2个重载

* `JwtBuilder setClaims(Claims claims);`

* `JwtBuilder setClaims(Map<String, Object> claims);`

  不能就是说，我们也可以直接传入map值对象

### 解析token

parseJwtToken方法是解析token

```java
public void parseJwtToken(String token) {
    try{

    }catch (Exception e){

    }
    Jws<Claims> jws = Jwts.parser().setSigningKey(SECRET).parseClaimsJws(token);
    String signature = jws.getSignature();
    Map<String, String> header = jws.getHeader();
    Claims Claims = jws.getBody();
}
```

