# [ARP Spoofing]
<img src="https://img-blog.csdnimg.cn/20210726194704703.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQxNTY0NA==,size_16,color_FFFFFF,t_70" width="400px">

## [Download]

```shell
go install github.com/ShangRui-hash/arp-spoofing-go@latest
```

## [Usage]
因为需要使用原始套接字，所以需要用户以root权限运行程序

```shell
/bin/bash > sudo arp-spoofing-go 
```

输入help ，查看可以使用的命令

```
阿弥陀佛 > help
- Commands:
  - clear              clear the screen
  - cut                通过ARP欺骗切断局域网内某台主机的网络
  - exit               exit the program
  - help               display help
  - hosts              主机管理功能
  - loot               查看嗅探到的敏感信息
  - middle-attack      中间人攻击
  - scan               扫描内网中存活的主机
  - set                配置参数
  - show               展示信息
  - sniff              嗅探用户名和密码
  - webspy             嗅探http报文
```

### show options
检查各项配置是否正确,如果配置不正确，可以使用 set key value 设置选项key的值为value
```
阿弥陀佛 > show options
+---------+-------------------+----------+----------------------+
|  NAME   |       VALUE       | REQUIRED |     DESCRIPTION      |
+---------+-------------------+----------+----------------------+
| ifname  |        en0        |   true   |     监听哪个网卡     |
+---------+-------------------+          +----------------------+
|  range  | 192.168.31.117/24 |          |       扫描范围       |
+---------+-------------------+          +----------------------+
| method  |        arp        |          | 扫描方式:all,arp,udp |
+---------+-------------------+          +----------------------+
| gateway |   192.168.31.1    |          |     局域网的网关     |
+---------+-------------------+----------+----------------------+
ARP Scan Options
```
### scan 扫描局域网中的主机
```
阿弥陀佛 > scan
[===================>] 99% 
```
### hosts 查看所有扫描到的主机
```
阿弥陀佛 > hosts
+----+----------------+-------------------+--------------------------------+
| ID |       IP       |        MAC        |            MACINFO             |
+----+----------------+-------------------+--------------------------------+
| 0  |  192.168.31.1  | ec:41:18:d9:a2:b9 |   XIAOMI Electronics,CO.,LTD   |
+----+----------------+-------------------+--------------------------------+
| 1  | 192.168.31.71  | 50:98:39:03:24:5e |  Xiaomi Communications Co Ltd  |
+----+----------------+-------------------+--------------------------------+
| 2  | 192.168.31.149 | ba:52:72:92:ea:f4 |                                |
+----+----------------+-------------------+--------------------------------+
| 3  | 192.168.31.168 | 8a:c4:6b:ac:25:49 |                                |
+----+----------------+-------------------+--------------------------------+
| 4  | 192.168.31.196 | 94:87:e0:37:af:37 |  Xiaomi Communications Co Ltd  |
+----+----------------+-------------------+--------------------------------+
| 5  | 192.168.31.200 | 50:5b:c2:c9:e8:d1 | Liteon Technology Corporation  |
+----+----------------+-------------------+--------------------------------+
| 6  | 192.168.31.222 | 3c:a5:81:94:43:25 | vivo Mobile Communication Co., |
|    |                |                   |              Ltd.              |
+----+----------------+-------------------+--------------------------------+
| 7  | 192.168.31.224 | 50:5b:c2:e9:e5:03 | Liteon Technology Corporation  |
+----+----------------+-------------------+--------------------------------+
```
### cut 向某台主机发送arp欺骗报文,启动后 发送数据包的协程将在后台默默运行
```
which host do you want to attack?
 ❯ 192.168.31.200
   192.168.31.222
   192.168.31.224
   192.168.31.1
   192.168.31.71
   192.168.31.149
   192.168.31.168
   192.168.31.196
```
```
Deceit gateway or target?
 ❯ gateway
   target
```
```
Send Reply packet or Request packet?
 ❯ request
   reply
```
```
+----------------+--------------+----------+--------+
|      目标      |     网关     | 欺骗方式 | 包类型 |
+----------------+--------------+----------+--------+
| 192.168.31.222 | 192.168.31.1 |  target  | reply  |
+----------------+--------------+----------+--------+
[*] 欺骗协程启动成功
[*] show cutted 查看当前正在被攻击的设备
```
### show cutted  查看当前正在被攻击的设备
```
阿弥陀佛 > show cutted
+----------------+
|  CUTTED HOSTS  |
+----------------+
| 192.168.31.222 |
+----------------+
```
### stop cut 停止发送
```
Please select host.
 ❯ 192.168.31.222
Stop cutting: 192.168.31.222
```

其他功能：
- webspy 可以嗅探所有流经本机网卡的http包,启动webspy前，建议向使用middleattack将目标主机的流量导过来
<img src="https://img-blog.csdnimg.cn/2021072619170764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQxNTY0NA==,size_16,color_FFFFFF,t_70"/>
- sniff 嗅探有敏感信息的http数据包，并存入redis，可以通过loot命令查看收集到的数据包

# [TODO] 

3. 优化webspy的功能
4. 检查中间人攻击模块 篡改数据包的mac地址是否确实篡改了
如果确实篡改了，为什么 wireshark 抓不到
5. 解决本主机在启动中间人攻击模块后上网慢的问题
6. 设置一个过滤器，只监听欺骗双方的数据包,查下过滤器的语法