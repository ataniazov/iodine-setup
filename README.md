# iodine-setup
Simple iodine DNS tunnel installation manual

## server side

```
# apt install iodine
```

```
# echo 1 > /proc/sys/net/ipv4/ip_forward
# iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
# iptables -A FORWARD -i eth0 -o dns0 -m state --state RELATED,ESTABLISHED -j ACCEPT
# iptables -A FORWARD -i dns0 -o eth0 -j ACCEPT
```

```
# iodined -fcP password 10.0.1.1 ns.yourdomain.com
```

```
vim /etc/default/iodine
...

# Default settings for iodine. This file is sourced from
# /etc/init.d/iodined
START_IODINED="true"
IODINED_ARGS="-c 10.0.1.1 ns.yourdomain.com"
IODINED_PASSWORD="password"

```

```
# systemctl unmask iodined.service
# systemctl enable iodined.service
# systemctl restart iodined.service
# systemctl status iodined.service
```


## client side

```
# apt install iodine
```

```
# iodine -fP password SERVER_IP ns.yourdomain.com
```

```
# route add SERVER_IP gw ROUTER_GATEWAY
# ip route replace default via 10.0.1.1
```
