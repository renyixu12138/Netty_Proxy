# Netty_Proxy
基于Netty实现的Tcp请求内网穿透,目前已测试SSH和Http均可以穿透。**服务端必须部署在公网服务器上，客户端部署在需要被穿透的内网服务器。**
## Server
启动命令:&emsp;    java -jar server.jar **-port 9527 -password 123lmy**

或者简写:&emsp;    java -jar server.jar **-p 9527 -pwd 123lmy**
- p     &emsp;  &emsp;  &emsp;     服务端端口
- pwd  &emsp;  &emsp; 服务端token校验
## Client
### http服务:
启动命令:&emsp;    java -jar client.jar **-host 47.96.158.922 -port 9527 -password 123lmy -proxy_host localhost -proxy_port 8080 -remote_port 12135**

或者简写: &emsp;   java -jar client.jar **-h 47.96.158.922 -p 9527 -pwd 123lmy -proxy_h localhost -proxy_p 8080 -remote_p 12135**
- h     &emsp;  &emsp;  &emsp;     服务端ip地址，可以是阿里云
- p &emsp;  &emsp;  &emsp;     服务端端口，如果是阿里云需要带控制台暴露该端口
- pwd  &emsp;  &emsp; 服务端token校验
- proxy_h &emsp;内网被代理的服务ip，因为客户端在内网，所以一般为localhost
- proxy_p &emsp;内网被代理的服务端口
- remote_p&ensp;需要公网，即服务端暴露访问内网应用的端口
### ssh服务:
&emsp;    java -jar client.jar -h 47.96.158.922 -p 9527 -password 123lmy -proxy_h localhost **-proxy_p 22 -remote_p 12222** 
- proxy_p 这是内网ssh连接端口，一般默认是22
- remote_p 服务端暴露访问ssh的端口
```java
如使用ssh连接工具输入以下即可连接成功:
	ip: 47.96.158.922
	port:12222
	username:xxx
	password:xxx
```

## 本地测试
### server:
 java -jar server.jar -port 9527 -password 123lmy
### client:
#### http：

java -jar client.jar -host localhost -port 9527 -password 123lmy -proxy_host localhost -proxy_port 8080 -remote_port 12135

---
测试http的web服务穿透:
	 &emsp; 本地开启一个web服务，暴露端口8080，在启动服务端和客户端之后,浏览器输入 localhost:12135即可访问到web应用。
#### ssh
  java -jar client.jar -h localhost -p 9527 -password 123lmy -proxy_h localhost -proxy_p 22 -remote_p 12222

---

测试ssh服务穿透:
	 &emsp; 在启动服务端和客户端之后,本地打开ssh连接工具，输入ip为localhost，端口为12222，再输入连接本机ssh的账号密码，即可连接到本机ssh。


## 心跳机制说明

由于客户端和服务端共用同一个超时机制，如果读写超时同时存在，就必须设置读超时大于写超时。

