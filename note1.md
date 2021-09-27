
## 查找子域名

查找子域名:
比如 https://grs.cup.edu.cn/D/index.jhtml 是 https://cup.edu.cn/ 的子域名

1. 子域名挖掘机，meltego
2. 搜索引擎挖掘，google中输入 site:qq.com
3. 第三方网站查询: tool.chinaz.com/subdomain， dnsdumpster.com
4. 证书透明度公开日志枚举: crt.sh， censys.io
5. 其他途径: phpinfo.me/domain， dns.aizhan.com


## FOFA资产收集

fofa.so， 同类型的有 shodan

* 通过 title body country 等规则等搜索
* 通过网站的 icon 图标搜索(根据 icon_hash， 搜索出使用同一 icon 的网站)
* 通过 js 文件查询(使用同一个 js 路径。开源CMS统一漏洞)
* 通过 规则集 来搜索


## 被动信息收集


## 主动信息收集

发现目标主机过程

1. 识别存活主机
2. 
3. 通过 234 层


基于 ping 命令的探测

* 查看经过的的网络设备 traceroute xuegod.cn

ping 延伸的命令，如 arping， fping， hping等


**arping**

* 内网协议
* arp协议: 发送前先根据 ip 地址查找 mac 地址， 并缓存起来

arp攻击: 冒充网关，使用 `arping 网关 -c 1` 查看是否有ip冲突

* arping 一次只能 ping 一个 ip
* 自动 ping 所有 ip. 使用 shell 脚本， 大致思路是通过 ifconfig 查找出 ip和mask， 然后 arping 所有主机


**netdiscover**

主动方式: 容易被管理员发现， `netdiscover -i eth0 -r 192.168.1.1/24`

被动方式: 更隐蔽，嗅探， `netdiscover -p -i wlan0`


**hping3**

* tcp/ip 数据包组装/分析工具，通常 web 服务会用来做压力测试使用，也可以进行 DOS 攻击实现
* 每个 hping 只能每次扫描一个目标.

`hping3 -h`

压力测试
```shell
hping3 -c 1000      数量
	-d 120     数据量
	-S	 只发送SYN包
	-w 64    window size
	-p 80    port
	--flood    洪水式
	--rand-source   出了路由器还是真实ip，内网内是伪造的ip
	xuegod.cn
```
该命令不要随意玩!!!


**fping**

ping 的加强版本，可以扫描一个ip段

`fping 192.168.1.1/24 -c 1`

## 基于 Nmap 扫描方式

一般使用：`nmap -sn xx 主机扫描，不扫描端口`



* 半连接扫描(不会留下记录)
nmap `-sS` xueshen.cn -p 80，81，8080，22，21，443
* 全连接扫描(会被扫描机器留下记录)
默认全连接扫描，`不加 -sS`



### nc 扫描端口

很强大，俗称"瑞士军刀"









































