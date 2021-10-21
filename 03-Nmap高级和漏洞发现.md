

## NMAP 高级使用

nmap 是一个网络探测和安全扫描程序,系统管理者和个人可以使用这个软件扫描大型的网络，获取那台主机正在运行以及提供什么服务等信息。

nmap 支持很多扫描技术，例如: UDP、 TCP connect()、TCP SYN（半开扫描）、ftp代理（bounce攻击）、反向标志、ICMP、FIN、ACK 扫描、圣诞树（Xmas Tree）、SYN 扫描和 null 扫描。还可以探测操作系统类型。

 <img src="images/03-Nmap高级和漏洞发现.assets/image-20211020213154834.png" alt="image-20211020213154834" style="zoom: 67%;" />



**端口状态说明**

`open`：应用程序在该端口接收TCP连接或者UDP报文。
`closed`：关闭的端口对于nmap也是可访问的，它接收 nmap探测报文并作出响应。但没有应用
程序在其上监听。
`filtered`：由于包过滤阻止探测报文到达端口，nmap无法确定该端口是否开放。过滤可能来自专业
的防火墙设备，路由规则或者主机.上的软件防火墙。
`unfiltered`：未被过滤状态意味着端口可访问，但是nmap无法确定它是开放还是关闭。只有用于
映射防火墙规则集的ACK扫描才会把端口分类到这个状态。
`open | filtered`：无法确定端口是开放还是被过滤，开放的端口不响应就是一个例子。没有响应也
可能意味着报文过滤器丢弃了探测报文或者它引发的任何反应。UDP，IP 协议，FIN，Null 等扫描会引起。
`closed | filtered`：(关闭或者被过滤的)无法确定端口是关闭的还是被过滤的



### NMAP 语法示例

`nmap 8.8.8.8` 扫描一台机器，默认扫描常规的 1000 个端口

**-v 显示冗余（verbosity）信息，再扫描国车过中显示扫描细节，从而让用户了解当前的扫描状态**

`nmap -v 8.8.8.8` 显示更详细的信息

**-p 指定端口**

`nmap -p 1-65536 localhost` 

`nmap -p 1-1000,8080,8888,6379 localhost`

生产环境中，关闭掉 80 以外的端口服务

```shell
lsof -i :22 # 查看 PID
ps -aux | grep pid # 查看进程启动所使用的文件的具体路径
```

**-sS 半连接扫描**

**-O 查看操作系统类型**

`nmap -sS -O cup.edu.cn`

**扫描网段**

`nmap -v 192.168.217.1/24` 根据掩码来扫描网段

`nmap -v -p 80 192.168.217.1-100` 查看一个网段 80 开放的主机

**--randomize_hosts  随即扫描，对目标主机进行随机扫描，而不是顺序的**

**--scan-delay  延时扫描，单位秒，调整扫描之间的演示**

`nmap -v --randomize_hosts --scan-delay 3000ms -p 80 192.168.217.1-200`

**使用统配符 * **

`namp -v 1.*.2.3-8` 一共扫描 254 * 6 个



### zenmap 图形化界面



**1、Intense Scan**

`namp -T4 -A xuegod.cn`

- `-T4` 有 6 个级别，`-T0-5`，级别越高扫描速度越快，也更容易被防火墙或 IDS 检测并屏蔽掉，推荐 `T4`
- `-A` 操作系统和软件版本号进行检测，并对目标进行 traceroute 路由探测。`-O` 仅识别目标操作系统



**2、Intense Scan Plus UDP**

`nmap -sS -sU -T4 -A -v`

- `-sS` 半连接扫描
- `-sU` UDP 扫描，比较慢



**3、Intense Scan, all TCP ports**

`nmap -p 1-65535 -T4 -A -v`



**4、Intense Scan, no ping**

`nmap -T4 -A -v -Pn`

- `-Pn` 非 ping 扫描



**5、Ping Scan**

`nmap -sn`

ping 扫描，速度快，容易被防火墙屏蔽，导致无扫描结果



**6、Quick Scan**

`nmap -T4 -F`

快速扫描



**7、Quick Scan Plus**

`nmap -sV -T4 -O -F --version-light`

- `-sV` 探测端口及版本信息
- `-O` 开启 OS 检测
- `--version-light` 设定侦测等级



**8、Quick traceroute**

`nmap -sn --traceroute`

- `-sn` ping 扫描，关闭端口扫描
- `-traceroute` 限制本机到目标的路由跃点



**9、Regular Scan** 常规扫描

`nmap ip`



**10、Slow comprehensive Scan**

`nmap -sS -sU -T4 -A -v -PE -PP -PS80,443 -PA3389 -PU40125 -PY -g 53 --script all cup.edu.cn`

慢速全面扫描





## 实战：DNMAP 分布式集群执行大规模扫描



dnmap 是一个用 python 写的进行分布式扫描的 nmap 扫描框架，我们可以用 dnmap 来通过多个台机器发起一个大规模的扫描，dnmap 采用 C/S 结构，执行大量扫描任务时非常便捷，扫描结果可以统一管理。



生成 pem 证书：

```shell
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out server.pem
```



dnmap 文档：https://www.kancloud.cn/haoyuanqiang/kali_linux_tools_documents/1060352

安装：使用 dnmap 安装包

文章：https://my.oschina.net/moziBlog/blog/3192770



**启动集群**

启动服务器

`dnmap_server -f nmap.txt -P server.pem`

- `-f` 指定文件，要执行 nmap 命令的文件
- `-P` 指定一个用于 TLS 连接的 pem 文件，默认是使用服务器提供的证书（不推荐）

客户端连接

`dnmap_client -s 192.168.18.10 -p 46001`

- `-s` 指定服务器 ip 地址
- `-p` 指定服务端端口，默认就是 46001



查看扫描结果，在服务端当前文件夹下的 `nmap_results/` 下

当扫描任务很大时，分布式集群扫描速度相对于单机更快

