# SpringBoot下Tomcat配置
-------------------------------------------------------------------------------------
## 修改默认端口
* server.port = 80
## 指定连接地址
* server.address = 127.0.0.1
## 错误处理
#### 关闭默认错误信息
* server.error.whitelabel.enabled = false
#### 设置错误页面地址
*  server.error.path = /user-error
#### 设置显示错误信息和堆栈跟踪
* server.error.include-exception = true
* server.error.include-stacktrace - always
## 服务器连接
#### 设置tomcat工作线程最大数量
* server.tomcat.max-threads = 200
#### 设置服务器连接超时
* server.connection-timeout = 5s
#### 定义请求头的最大大小 
* server.max.http-header-size = 8kb
#### 定义请求正文的最大大小
* server.tomcat.max-swawllow-size = 2MB
#### 定义PSOT数据的大小
* server.tomcat.max-http-post-size = 2MB
## SSL支持
#### 启用SSL支持
* server.ssl.enabled = true
* server.ssl.protocol = TLS
#### 配置保存证书秘钥库中秘钥的别名
* server.ssl.key-store-password = my_password
* server.ssl.key-store-type = keystore_type
* server.ssl.key-store = keystore-path
* server.ssl.key-alias=tomcat
## Tomcat服务器访问日志
#### 启用访问日志
* server.tomcat.accesslog.enabled = true 
#### 文件路径
* server.tomcat.accesslog.diretory=logs
#### 日期格式
* server.tomcat.accesslog.file-date-format=yyyy-mm-dd
#### 前缀
* server.tomcat.accesslog.prefix=access_log
#### 后缀
* server.tomcat.accesslog.suffix=.log