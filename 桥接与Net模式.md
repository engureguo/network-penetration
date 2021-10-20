### 虚拟机连接局域网

https://blog.csdn.net/daxueba/article/details/56278322

https://www.cnblogs.com/wdh1995/p/7137851.html



linux 使用无线网卡

https://zhidao.baidu.com/question/581139349.html



NET模式的网卡桥接至无线网卡

https://blog.csdn.net/a7320760/article/details/8296008



桥接模式

1. 虚拟机上添加桥接模式的网络，桥接至无线网卡

   VM上桥接模式的网络最多只能配置一个

2. 配置虚拟机网卡为桥接模式，或者指定 1. 中配置的虚拟网络，两个都可以

   复制物理网络连接状态还不知道怎么用，不选

3. 虚拟机上新建网卡，分别配置无线网卡的 ip、gateway、mask、dns

   此时有网卡的物理机可能上不了网。虚拟机可以上网。



桥接模式使用 dhcp？

> https://www.cnblogs.com/haoabcd2010/p/8683656.html
>
> > 物理主机虚拟出一个“交换机”，虚拟机和物理机都连接在这个交换机上，端口不同互不干扰
>
> 
>
> 实验测试连接校园网（身份认证）虚拟机自动连接时自动分配 ip 地址，可以 ping 其他主机，不能上网。
>
> 网络中心：一个 mac 找到了两个 ip ？？（旧 ip 租赁时间未到）
>
> 
>
> ip 租赁时间未到，实体机校园网下线，虚拟机指定 ip 连接校园网，请求过后无法收到结果，刷新后可以上网，用的是实体机的 mac 地址。
>
> 网络中心：接受【旧 ip + 旧 mac】，将结果发送到该主机，虚拟机收不到。
>
> 可以正常上网，说明虚拟机可以接受响应。
>
> 
>
> 其他链接
>
> https://www.cnblogs.com/crmn/articles/9022862.html



linux 安装 usb 无线网卡

https://www.wyr.me/post/623##toc2-1

https://blog.51cto.com/jiangkun08/1284327

https://www.cnblogs.com/zhangjiankun/p/4888956.html



桥接模式原理

https://www.cnblogs.com/ck1020/p/5889570.html



编译驱动到内核

https://www.cnblogs.com/downey-blog/p/10600249.html



问题解决：kali2021安装 realtek 8811cu

https://blog.csdn.net/Zero_D_Slayer/article/details/117856681



