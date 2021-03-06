## 一、Nginx优点：

1、工作在网络7层之上，可针对http应用做一些分流的策略，如针对域名、目录结构，它的正规规则比HAProxy更为强大和灵活，所以，目前为止广泛流行。

2、Nginx对网络稳定性的依赖非常小，理论上能ping通就能进行负载功能。

3、Nginx安装与配置比较简单，测试也比较方便，基本能把错误日志打印出来。

4、可以承担高负载压力且稳定，硬件不差的情况下一般能支撑几万次的并发量，负载度比LVS小。

5、Nginx可以通过端口检测到服务器内部的故障，如根据服务器处理网页返回的状态码、超时等，并会把返回错误的请求重新提交到另一个节点。

6、不仅仅是优秀的负载均衡器/反向代理软件，同时也是强大的Web应用服务器。LNMP也是近些年非常流行的Web架构，在高流量环境中稳定性也很好。

7、可作为中层反向代理使用。

8、可作为静态网页和图片服务器。

9、Nginx社区活跃，第三方模块非常多，相关的资料在网上比比皆是。


Nginx缺点：

1、适应范围较小，仅能支持http、https等协议。

2、对后端服务器的健康检查，只支持通过端口检测，不支持url来检测。

 
## 二、LVS优点：

1、抗负载能力强、是工作在网络4层之上仅作分发之用，没有流量的产生，这个特点也决定了它在负载均衡软件里的性能最强的，对内存和cpu资源消耗比较低。

2、配置性比较低，这是一个缺点也是一个优点，因为没有可太多配置的东西，所以并不需要太多接触，大大减少了人为出错的几率。

3、工作稳定，因为其本身抗负载能力很强，自身有完整的双机热备方案，如LVS+Keepalived，不过我们在项目实施中用得最多的还是LVS/DR+Keepalived。

4、无流量，LVS只分发请求，而流量并不从它本身出去，这点保证了均衡器IO的性能不会收到大流量的影响。

5、应用范围比较广，因为LVS工作在4层，所以它几乎可以对所有应用做负载均衡，包括http、数据库、在线聊天室等等。


LVS的缺点：

1、软件本身不支持正则表达式处理，不能做动静分离；而现在许多网站在这方面都有较强的需求，这个是Nginx/HAProxy+Keepalived的优势所在。

2、如果是网站应用比较庞大的话，LVS/DR+Keepalived实施起来就比较复杂了，特别后面有Windows Server的机器的话，如果实施及配置还有维护过程就比较复杂了，相对而言，Nginx/HAProxy+Keepalived就简单多了。


## 三、HAProxy优点：

1、HAProxy是支持虚拟主机的，可以工作在4、7层(支持多网段)

2、HAProxy的优点能够补充Nginx的一些缺点，比如支持Session的保持，Cookie的引导；同时支持通过获取指定的url来检测后端服务器的状态。

3、HAProxy跟LVS类似，本身就只是一款负载均衡软件；单纯从效率上来讲HAProxy会比Nginx有更出色的负载均衡速度，在并发处理上也是优于Nginx的。

4、HAProxy支持TCP协议的负载均衡转发，可以对MySQL读进行负载均衡，对后端的MySQL节点进行检测和负载均衡，大家可以用LVS+Keepalived对MySQL主从做负载均衡。


HAPorxy缺点：

1. 不支持POP/SMTP协议

2. 不支持SPDY协议

3. 不支持HTTP cache功能。现在不少开源的lb项目，都或多或少具备HTTP cache功能。

4. 重载配置的功能需要重启进程，虽然也是soft restart，但没有Nginx的reaload更为平滑和友好。

5. 多进程模式支持不够好