---
description: An alternative way to establish TCP tunnel or port forwarding for frp.
---

# Create TCP Tunnel/Port Forwarding by SSH

### Remote Port Forwarding

The request `http://example.com:8080` will be passed to `192.168.0.101:10080`.

![](.gitbook/assets/image%20%2814%29.png)

```bash
# -N: Do not execute a remote command. This is useful for just forwarding ports
# -R: Specifies that the given port on the remote (server) host is to be forwarded to the given host and port on the local side.
ssh -N -R 8080:192.168.0.101:10080 <username>@example.com
```

### 

### Local Port Forwarding

The request `http://192.168.0.100:10080` will be passed to `10.0.0.161:8080`

```bash
# -N: Do not execute a remote command. This is useful for just forwarding ports
# -L: Specifies that the given port on the local (client) host is to be forwarded to the given host and port on the remote side. 
ssh -N -L 10080:example.com:8080 <username>@example.com
```

![](.gitbook/assets/image%20%2812%29.png)

### Notes

You can change target ip/domain name to any other address, such as localhost or other location that server can access.

ssh -N -R 8080:localhost:10080 &lt;username&gt;@example.com  
ssh -N -R 8080:google.com:80 &lt;username&gt;@example.com

are both valid.

### References

[https://linux.die.net/man/1/ssh](https://linux.die.net/man/1/ssh)

