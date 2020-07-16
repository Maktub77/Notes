#### Logstash日志采集

2. ###### 规定输出审计信息日志格式

   ```
   //IP:接口调用ip userId:用户Id userName:用户名 organizationId:组织机构ID //organizationName:组织机构名 operationDigest：接口唯一中文定义标识（例如登录）
   
   log.info(String.format("审计信息:[IP=%s] [userId=%s] [userName=%s] [organizationId=%s] [organizationName=%s] [operationDigest=%s]",
                       request.getHeader("X-Forwarded-For"),
                       oauth2Context.getUserId(),
                       oauth2Context.getUsername(),
                       oauth2Context.getOrganizationId(),
                       oauth2Context.getOrganizationName(),
                       annotation != null ? ((ApiOperation) annotation).value() : request.getRequestURI()
   ));
   ```

   日志输出示例

   ```
   [2019-08-23 16:04:58] INFO  35 preHandle - 审计信息:[IP=null] [userId=217529949458927616] [userName=admin] [organizationId=217536477956018176] [organizationName=市政府办公厅] [operationDigest=判断当前用户密码是否有效]
   ```
   配置日志字段解释

   |       参数       |               说明               |
   | :--------------: | :------------------------------: |
   |        IP        |            接口调用ip            |
   |      userId      |              用户Id              |
   |     userName     |              用户名              |
   |  organizationId  |            组织机构ID            |
   | organizationName |            组织机构名            |
   | operationDigest  | 接口唯一中文定义标识（例如登录） |

3. ###### 安装Logstash 

   * 版本：

   在Logstash安装目录`/config`下新建文件`patterns`

   ```
   INFO [-a-zA-Z0-9/\.:? = \u4e00-\u9fa5]+
   LOGDATE %{YEAR}[/-]%{MONTHNUM}[/-]%{MONTHDAY}
   LOGTIME %{HOUR}[/:]%{MINUTE}[/:]%{SECOND}
   SJMESSAGE %{SJTAG:message}:\[%{INFO:IP}\] \[%{INFO:user_id}\] \[%{INFO:user_name}\] \[%{INFO:organization_id}\] \[%{INFO:organization_name}\] \[%{INFO:operationd_igest}\]
   SJTAG 审计信息
   MESSAGE .*
   
   TJMESSAGE %{TJXX:message}:\[%{INFO:app_key}\] \[%{INFO:api_id}\] \[%{INFO:target_url}\] %{MESSAGE:data}
   TJXX 统计信息
   ```

   在Logstash安装目录`/config`下新建配置文件`logstash.conf`

   ```conf
   input {
      	#从本地路径读取日志文件
          file {
          	#日志路径 （需要配置为本地输出日志路径）
              path => "/IDEA/logs/log.log" 
          }
      }
      
      filter {
      	#过滤保留符合审计信息的日志
      	if([message] !~ ".*审计信息:\[IP=.*\] \[userId=.*\] \[userName=.*\] \[organizationId=.*\] \[organizationName=.*\] \[operationDigest=.*\].*"){
      		drop{}
      	}
          grok {
          #匹配到指定的patterns （需要配置为本地logstash对应路径）
            patterns_dir => ["/ELK/logstash-7.3.0/config/patterns"]
            match => { "message" => "\[(%{INFO:project_name})\] \[(%{INFO:request_id})\] %{LOGDATE:logdate} %{LOGTIME:logtime} %{LOGLEVEL:loglevel} \[%{INFO:thread}\] %{JAVACLASS:doclass} \[%{INFO:javafile}\] (%{SJMESSAGE}|%{MESSAGE:message})" }
          }
      }
      
      output {
      	#输出到elasticsearch
          elasticsearch {
          	#输出地址 （需要配置为对应elasticsearch输出地址）
              hosts => ["http://localhost:9200"]
              #不同厂商建立不同索引,格式为"厂商名-%{+YYYY.MM.dd}"
              index => "PY-%{+YYYY.MM.dd}"
          }
          stdout {
          }
      }
   ```

   elasticsearch输出地址

   | 环境 | 输出地址 |
   | ---- | -------- |
   |      |          |
   |      |          |

   厂商名建立索引

   | 厂商名 | 对应index |
   | ------ | --------- |
   |        |           |
   |        |           |

 在安装目录`\bin`下 指定配置文件运行

   ```
   logstash.bat -f ../config/logstash.conf
   ```

   

