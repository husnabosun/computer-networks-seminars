SECTION 4 — TCP vs UDP PRACTICAL COMPARISON

## TASK-1 — TCP Connection with Netcat

---

### Setup

```bash
# 1st terminal (Server)
nc -l -p 9000

# 2nd terminal (Client)
nc 127.0.0.1 9000
```

### Output

```
hey everyone
dilerim ki mutlu old sevgilim ben olmasam bile hayat gülsün sana günahım boynuna ağlayan bir çift göz bıraktın arkanda
```

> The written text is successfully sent from the client side and can be seen by both server and client.

### Conclusion

> **TCP is a connection-oriented protocol.** When you started the client and server, they performed a **"Three-Way Handshake"** to agree on the connection.
>
> It is also **stream-oriented**. It acts like a phone call — as long as neither side hangs up, the data continues to flow. In `netstat` this connection shows as **ESTABLISHED**.

---

## TASK-2 — UDP Connection with Netcat

---

### Setup

```bash
# 1st terminal (Server)
nc -u -l -p 9001

# 2nd terminal (Client)
echo "test udp" | nc -u 127.0.0.1 9001
```

### Output

```
test udp    ← seen as output on server side (1st message)
            ← no output seen on server side (2nd message)
```

> On the first send, `test udp` appears on the server. When a message is sent for the **2nd time**, no output is seen on the server side.

### Conclusion

> **UDP is a connectionless protocol.** It doesn't care if the other side is listening or ready — it just throws the packet into the network.
>
> By default, the Netcat server **closes itself after receiving exactly one UDP packet**, which is why the second message produces no output.
