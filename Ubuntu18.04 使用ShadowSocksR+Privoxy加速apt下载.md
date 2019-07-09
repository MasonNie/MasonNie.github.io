##安装ShadowsocksR
```
如果没有git，先装git
sudo apt-get install git

用脚本安装
wget https://github.com/the0demiurge/CharlesScripts/raw/master/charles/bin/ssr

将脚本放入/usr/local/bin中，方便平时调用

sudo mv ssr /usr/local/bin
sudo chmod 766 /usr/local/bin/ssr

安装SSR
ssr install
```

###安装完成后配置SSR
```
ssr config
```
会得到大概这样的，然后修改成你的配置
```
{
    "server": "{your-server}",
    "server_ipv6": "::",
    "server_port": 12345,
    "local_address": "127.0.0.1",
    "local_port": 1080,

    "password": "{your-password}",
    "method": "aes-256-cfb",
    "protocol": "origin",
    "protocol_param": "",
    "obfs": "plain",
    "obfs_param": "",
    "speed_limit_per_con": 0,
    "speed_limit_per_user": 0,

    "additional_ports" : {}, // only works under multi-user mode
    "additional_ports_only" : false, // only works under multi-user mode
    "timeout": 120,
    "udp_timeout": 60,
    "dns_ipv6": false,
    "connect_verbose_info": 0,
    "redirect": "",
    "fast_open": false
}
```
##安装配置Privoxy
```
sudo apt-get install privoxy  #安装
sudo nano /etc/privoxy/config  #配置

找到listen-address 改成：  #端口可自选
listen-address 127.0.0.1:8118
listen-address [::1]:8118

找到forward-socks5t，没有就加一行：
 #特别注意后边有点是必须的，代表不进行二次代理
forward-socks5t   /         127.0.0.1:1080 .   #将socks转为http
特别注意后边有点是必须的，代表不进行二次代理

重启服务
sudo service privoxy restart

```
**P.S. 如果报错有可能是没启动ipv6**
##配置全局代理
```
export http_proxy="http://127.0.0.1:8118/" 
export https_proxy="http://127.0.0.1:8118/"
```
**取消代理**
```
unset http_proxy
unset https_proxy
```
**ping不通是因为，ping命令发送ICMP报文，代理服务器不会转发，所以会block。
替代的可以用curl来测试是否配置好了代理服务**

##Proxychain4
```
sudo apt install proxychains4

sudo nano /etc/proxychains4.conf

将
socks4  127.0.0.1 9050
改为
socks5  127.0.0.1 1080
```

## *Ref*
- [Ubuntu Server 18.04 LTS 使用Shadowsocks-ShadowsocksR访问互联网](https://mystery0.vip/2018/08/23/Ubuntu%20Server%2018.04%20LTS%20%E4%BD%BF%E7%94%A8Shadowsocks-ShadowsocksR%E8%AE%BF%E9%97%AE%E4%BA%92%E8%81%94%E7%BD%91/)
- [linux下配置全局代理](https://www.jianshu.com/p/2bb0b27226c9)
