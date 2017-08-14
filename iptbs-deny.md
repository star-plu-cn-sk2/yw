# iptables限制HOST访问其它主机权限

## 项目背景
- 内网主机暴露在公网，给外包人员访问，通过此策略隔离与内网其它主机权限，保障内网安全性。

``` bash
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT DROP [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT

#允许服务器自己的SSH
-A INPUT -s 192.168.1.1/32 -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

-A INPUT -s 127.0.0.1/32 -d 127.0.0.1/32 -j ACCEPT
-A INPUT -s 192.168.2.5/32 -d 192.168.2.5/32 -j ACCEPT
-A OUTPUT -s 127.0.0.1/32 -d 127.0.0.1/32 -j ACCEPT
-A OUTPUT -s 192.168.2.5/32 -d 192.168.2.5/32 -j ACCEPT

-A OUTPUT -p tcp --sport 22 -j ACCEPT

#允许服务器SSH到其他机器
#-A OUTPUT -p tcp --dport 22 -j ACCEPT   

-A OUTPUT -p tcp --sport 80 -j ACCEPT
#-A OUTPUT -p tcp --dport 80 -j ACCEPT

-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
```
