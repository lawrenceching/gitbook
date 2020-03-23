# Expose RDP service by SSH port forwarding

From time to time, we need to expose our local Windows to the outside of the world. So that we can access the machine from anywhere through the Internet. However we may not get a public ip from ISP. So we wanna setup a TCP tunnel between local Windows and public server.

And here is how it goes.

### Step 1: Turn on GatewayPorts

Modify `/etc/ssh/sshd_config` , set `GatewayPorts no` to **yes**. That will allow sshd to listen on other internet interface like 0.0.0.0. If it's no, sshd will only listen to loopback which means the external requests cannot come in.

### **Step 2: Establish Port Forwarding**

Run command in local Windows. Change `user@us1.example.com` to the real server of yours.

```text
# ssh -N -R 0.0.0.0:5200:<rdp-host>:3389 <user>@<internet-host>
ssh -N -R 0.0.0.0:5200:localhost:3389 user@us1.example.com
```

### **Step 3: Open port on firewalld**

You will need to open port 5200 on firewalld in order to let external requests come in.

```text
sudo firewall-cmd --permanent --add-port=5200/tcp
sudo firewall-cmd --reload
```

### **Step 4: Managed by systemd\(Optional\)**

If you wanna make service stable, it's a common way to manage service by systemd. It provides the features like auto-startup, auto-restart and log management.

Create `/etc/systemd/system/ssh_rdp_forwarding.service` 

```text
# /etc/systemd/system/ssh_rdp_forwarding.service
[Unit]
Description=A SSH port forwarding for exposing RDP to the Internet

[Service]
Type=simple
ExecStart=/usr/bin/ssh -N -R 0.0.0.0:5200:10.0.0.194:3389 root@edge.imlc.me

[Install]
WantedBy=multi-user.target
```

And then run `systemctl enable ssh_rdp_forwarding` and `systemctl start ssh_rdp_forwarding` to start the service.



Now things get ready. You can rdp to your local Windows by connecting to `us1.example.com:5200`

![](.gitbook/assets/image%20%2814%29.png)



