# SECTION 1 — BASIC CONNECTIVITY (PING TESTS)

## TASK-1

---

### COMMAND 1

> By throwing 4 packages to google server, I tested my internet connectivity and latency.

```bash
ping -c 4 google.com
```

### OUTPUT 1

```
PING google.com (142.251.127.102) 56(84) bytes of data.
64 bytes from lcfrai-in-f102.1e100.net (142.251.127.102): icmp_seq=1 ttl=102 time=100 ms
64 bytes from lcfrai-in-f102.1e100.net (142.251.127.102): icmp_seq=2 ttl=102 time=169 ms
64 bytes from lcfrai-in-f102.1e100.net (142.251.127.102): icmp_seq=3 ttl=102 time=226 ms
64 bytes from lcfrai-in-f102.1e100.net (142.251.127.102): icmp_seq=4 ttl=102 time=80.8 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 80.759/144.099/225.974/57.635 ms
```

---

### COMMAND 2

> By throwing ping to my computers' local IP address, I verified my network card and network stack is working without errors.

```bash
ping -c 4 172.19.99.185
```

### OUTPUT 2

```
PING 172.19.99.185 (172.19.99.185) 56(84) bytes of data.
64 bytes from 172.19.99.185: icmp_seq=1 ttl=64 time=0.496 ms
64 bytes from 172.19.99.185: icmp_seq=2 ttl=64 time=0.044 ms
64 bytes from 172.19.99.185: icmp_seq=3 ttl=64 time=0.051 ms
64 bytes from 172.19.99.185: icmp_seq=4 ttl=64 time=0.051 ms
```

---

### COMMAND 3

```bash
ping -c 4 192.168.1.1
```

### OUTPUT 3

```
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.

--- 192.168.1.1 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3098ms
```

> Since there is no device with such IP address in my current network, the test resulted with 100% package loss. **THIS ADDRESS IS NOT REACHABLE IN MY CURRENT NETWORK ENVIRONMENT.**

---

# SECTION 2 — DNS & ADDRESS INVESTIGATION (nslookup)

## TASK-2

---

### COMMAND 1 — FOR ESTABLISHED

> When 2 terminals connected as Netcat and TCP, three-way handshaking is done and the status is ESTABLISHED.

```bash
# CLIENT
nc 127.0.0.1 9101

# SERVER (on another terminal)
nc -l -p 9100
```

```
hey
o yeahh    ← THREE-WAY HANDSHAKE WORKS PROPERLY
```

```bash
netstat -tunp
```

```
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:9100          127.0.0.1:37400         ESTABLISHED 1114/nc
tcp        0      0 127.0.0.1:37400         127.0.0.1:9100          ESTABLISHED 1115/nc
```

---

### COMMAND 2 — FOR LISTENED

> When I start Netcat on the server terminal, I saw the system was waiting on port 9191.

```bash
netstat -tulnp
```

```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:9100            0.0.0.0:*               LISTEN      1114/nc
tcp        0      0 10.255.255.254:53       0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -
udp        0      0 10.255.255.254:53       0.0.0.0:*                           -
udp        0      0 127.0.0.1:323           0.0.0.0:*                           -
udp6       0      0 ::1:323                 :::*
```

---

### COMMAND 3 — FOR UDP

> **Unlike TCP, UDP is a connectionless protocol, so the 'STATE' column remains blank. It indicates that the system is ready to receive independent datagrams on this port.**

```bash
# SERVER (on another terminal)
nc -u -l -p 9101

# CLIENT
nc 127.0.0.1 9100
```

```bash
netstat -tulnp | grep udp
```

```
udp        0      0 0.0.0.0:9101            0.0.0.0:*                           1151/nc
# 1151/nc IS WAITING OPEN — SERVER IS LISTENING
```

```bash
# CLIENT THREW A TEST MESSAGE TO SERVER
echo "test" | nc -u 127.0.0.1 9101
```

```bash
netstat -tulnp | grep udp
```

```
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -
udp        0      0 10.255.255.254:53       0.0.0.0:*                           -
udp        0      0 127.0.0.1:323           0.0.0.0:*                           -
udp6       0      0 ::1:323                 :::*
# SERVER IS CLOSED. THE WORK IS DONE
```

---

# SECTION 3 — NETWORK STATES & NETCAT (TCP/UDP ANALYSIS)

> The **NXDOMAIN** (Non-Existent Domain) error confirms that the DNS server was reachable but could not find a registered record for this name.

```bash
nslookup robotistan.com
```

```
Server:         10.255.255.254
Address:        10.255.255.254#53

Non-authoritative answer:
Name:   robotistan.com
Address: 104.26.2.247
Name:   robotistan.com
Address: 104.26.3.247
Name:   robotistan.com
Address: 172.67.70.250
```

```bash
nslookup hösnelelelelybüs.com
```

```
Server:         10.255.255.254
Address:        10.255.255.254#53

** server can't find xn--hsnelelelelybs-vpb2i.com: NXDOMAIN
```
