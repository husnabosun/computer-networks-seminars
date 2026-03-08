# SECTION 4 — TCP vs UDP PRACTICAL COMPARISON

## TASK-1

---

### 1st terminal (Server)

```bash
nc -l -p 9000
```

### 2nd termina (Client)

```bash
nc 127.0.0.1 9000
```

### Output

```
hey everyone
dilerim ki mutlu old sevgilim ben olmasam bile hayat gülsün sana günahım boynuna ağlayan bir çift göz bıraktın arkanda
```

> The written text is succesfully sent from client side and can be seen by both server and client

### CONCLUSION

> TCP is a connection-oriented protocol. When you started the client and server, they performed a "Thee way handshake" to agree on the connection.
> It is also "stream oriented". It acts like a phone call, as long as neither side hangs up, the data continues to flow. In netstat this shows ESTABLISHED.

---

## TASK-2

---

### 1st terminal (Server)

```bash
nc -u -l -p 9001
```

### 2nd terminal (Client)

```bash
echo "test udp" | nc -u 127.0.0.1 9001
```

### Output

```
test udp (seen as an output on server side)
```

> when a message sent for the 2nd time no output is seen on server side

### CONCLUSION

> UDP is a connectionless protocol. It doesn't care if the other side is listenining or ready. It just throws the packet into the network. By default the netcat server closes itself after receivng exactly one UDP packet
