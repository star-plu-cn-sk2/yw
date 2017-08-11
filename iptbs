:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT DROP [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT

#允许服务器自己的SSH
-A INPUT -s 192.168.162.69/32 -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -s 106.75.176.229 -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

-A INPUT -s 127.0.0.1/32 -d 127.0.0.1/32 -j ACCEPT
-A INPUT -s 192.168.102.55/32 -d 192.168.102.55/32 -j ACCEPT
-A OUTPUT -s 127.0.0.1/32 -d 127.0.0.1/32 -j ACCEPT
-A OUTPUT -s 192.168.102.55/32 -d 192.168.102.55/32 -j ACCEPT

-A OUTPUT -p tcp --sport 22 -j ACCEPT

#允许服务器SSH到其他机器
#-A OUTPUT -p tcp --dport 22 -j ACCEPT   

-A OUTPUT -p tcp --sport 80 -j ACCEPT
#-A OUTPUT -p tcp --dport 80 -j ACCEPT

-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
