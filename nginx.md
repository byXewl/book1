nginx是一个具有高性能的http和反向代理的web服务器，同是是一个邮件协议（POP3/STMP/IMAP）代理服务器，c语言编写，源码开源（一般基于lua语言二次开发）。


## **功能：**
部署网站
静态资源服务器
url重写
重定向
反向代理：默认轮询访问不同服务器，可自己设置权重。（如果要解决session问题，加ip_hash,一个用户访问一直是同一台服务器。token不用考虑分布式）
>正向代理：对目标服务器隐藏用户，如实现跨域，fq等。
>反向代理：对用户隐藏目标服务器，
                    如转发给不同服务器实现负载均衡。
                    如内网访问后端服务器，让后端服务器不外网开放，保证安全。
<br>
## **常用命令：**
```
cd /usr/local/nginx/sbin/
./nginx
./nginx -s stop 停止退出（所有监听站点停止）
./nginx -s quit  安全退出 （所有监听站点停止）
./nginx -s reload 重新加载配置文件(改了配置文件就要重载)
```
```
ps aux|grep nginx 
ps -ef|grep nginx
查看nginx进程:
master主进程
work工作进程
```

lsof -i :80
查看80端口被占用情况，主进程和工作进程都在监听80端口。
>在传统的操作系统中，一个进程通常与一个程序相对应，但是在网络编程中，允许多个进程绑定到同一个端口并不会造成冲突。这是因为在网络编程中，进程不仅仅与程序相对应，还与一个网络套接字相关联。

>一个网络套接字是由IP地址、端口号和协议类型（如TCP或UDP）唯一标识的通信端点。多个进程可以在同一台计算机上创建不同的套接字并绑定到相同的IP地址和端口号上。这是通过套接字选项 `SO_REUSEADDR` 实现的。


## **做网关功能：**


### 使用 Spring Cloud 网关组件的情况：
1. **Spring Cloud Gateway**：这是Spring Cloud中较新的网关实现，它提供了灵活的路由、过滤器等功能，可以作为微服务架构的网关。Spring Cloud Gateway可以处理服务发现、路由、限流、安全等。

### 是否需要Nginx作为网关的考虑：
1. **特殊需求**：有时候，一些特定的需求可能需要 Nginx 提供的某些功能，比如静态文件服务、反向代理、缓存控制等。在这种情况下，你可能会考虑保留 Nginx。
2. **遗留系统**：如果你的系统已经在 Nginx 上部署，并且在微服务架构下逐步进行迁移，你可能会选择在转换过程中保留 Nginx 作为网关，以便逐步迁移服务。

