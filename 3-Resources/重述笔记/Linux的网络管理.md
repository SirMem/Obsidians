---
Project: "[[Linux|Linux]]"
Status: 🟩
tags:
  - Resources
Deadline: 
CreateTime: 2024-06-03
Connected: 
---

好的，接下来我们将详细讲解Linux中的网络管理，包括基本的网络命令、SSH连接和配置等。

### 基本的网络命令

#### `ifconfig` 命令

`ifconfig`（interface configuration）命令用于配置和显示网络接口信息。

基本用法：
```bash
ifconfig
```

示例：

1. 显示所有网络接口的配置信息：
   ```bash
   ifconfig
   ```

2. 配置网络接口（需要root权限）：
   ```bash
   sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0 up
   ```

#### `ip` 命令

`ip` 命令是一个功能更强大的网络配置工具，取代了`ifconfig`。

基本用法：
```bash
ip [选项] 对象 命令
```

示例：

1. 显示所有网络接口的信息：
   ```bash
   ip addr show
   ```

2. 配置网络接口（需要root权限）：
   ```bash
   sudo ip addr add 192.168.1.10/24 dev eth0
   ```

3. 显示路由表：
   ```bash
   ip route show
   ```

#### `ping` 命令

`ping` 命令用于测试主机之间的网络连接。

基本用法：
```bash
ping [选项] 目标主机
```

示例：

1. 测试与`google.com`的连接：
   ```bash
   ping google.com
   ```

2. 发送指定次数的ICMP请求：
   ```bash
   ping -c 4 google.com
ping   ```

#### `traceroute` 命令

`traceroute` 命令用于显示到目标主机的路径。

基本用法：
```bash
traceroute [选项] 目标主机
```

示例：

1. 显示到`google.com`的路径：
   ```bash
   traceroute google.com
   ```

#### `netstat` 命令

`netstat`（network statistics）命令用于显示网络连接、路由表、接口统计信息等。

基本用法：
```bash
netstat [选项]
```

示例：

1. 显示所有活动的网络连接：
   ```bash
   netstat -a
   ```

2. 显示所有监听端口：
   ```bash
   netstat -l
   ```

3. 显示路由表：
   ```bash
   netstat -r
   ```

#### `ss` 命令

`ss` 命令是`netstat`的一个更快的替代品，用于显示套接字统计信息。

基本用法：
```bash
ss [选项]
```

示例：

1. 显示所有TCP连接：
   ```bash
   ss -t
   ```

2. 显示所有监听端口：
   ```bash
   ss -l
   ```

### SSH 连接和配置

#### SSH 基本介绍

SSH（Secure Shell）是一种用于远程登录和其他网络服务的安全协议，通常用于在不安全的网络上提供安全的加密通信。

#### 使用 `ssh` 命令连接到远程服务器

基本用法：
```bash
ssh [选项] 用户名@主机
```

示例：

1. 连接到远程服务器：
   ```bash
   ssh user@remote_host
   ```

2. 指定端口连接：
   ```bash
   ssh -p 2222 user@remote_host
   ```

3. 使用密钥文件连接：
   ```bash
   ssh -i /path/to/private_key user@remote_host
   ```

#### 配置 SSH

SSH的配置文件位于`/etc/ssh/sshd_config`。编辑该文件可以配置SSH服务器的行为。

示例：

1. 修改SSH默认端口：
   ```plaintext
   Port 2222
   ```

2. 禁止root用户登录：
   ```plaintext
   PermitRootLogin no
   ```

3. 仅允许特定用户登录：
   ```plaintext
   AllowUsers user1 user2
   ```

修改配置文件后，需要重启SSH服务以应用更改：
```bash
sudo systemctl restart sshd
```

#### 生成 SSH 密钥对

生成SSH密钥对用于无密码登录和提高安全性。

基本用法：
```bash
ssh-keygen -t rsa -b 4096
```

示例：

1. 生成一个新的RSA密钥对：
   ```bash
   ssh-keygen -t rsa -b 4096
   ```

2. 生成一个新的ECDSA密钥对：
   ```bash
   ssh-keygen -t ecdsa -b 521
   ```

生成密钥对后，将公钥复制到远程服务器：
```bash
ssh-copy-id user@remote_host
```

或者手动将公钥添加到远程服务器的`~/.ssh/authorized_keys`文件中。

#### SSH 隧道

SSH隧道用于在不安全的网络上创建加密的通信通道。

基本用法：
```bash
ssh -L 本地端口:目标主机:目标端口 用户名@远程主机
```

示例：

1. 创建一个本地到远程的SSH隧道：
   ```bash
   ssh -L 8080:remote_host:80 user@remote_host
   ```

2. 创建一个远程到本地的SSH隧道：
   ```bash
   ssh -R 9090:localhost:80 user@remote_host
   ```


### `iptables` 防火墙配置

`iptables` 是Linux内核防火墙的用户空间工具，用于配置规则来过滤和操作网络流量。

#### 基本用法

`iptables`命令有四个表和五个链。表包括`filter`（默认表）、`nat`、`mangle`和`raw`。链包括`INPUT`、`FORWARD`、`OUTPUT`、`PREROUTING`和`POSTROUTING`。

#### 常见命令

1. **查看规则**：
   ```bash
   sudo iptables -L
   ```

2. **允许特定端口的流量**（如SSH的22端口）：
   ```bash
   sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
   ```

3. **阻止特定IP地址**：
   ```bash
   sudo iptables -A INPUT -s 192.168.1.100 -j DROP
   ```

4. **删除规则**：
   ```bash
   sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT
   ```

5. **保存规则**：
   不同的发行版有不同的方法保存规则，以便重启后仍然生效。
   - 在Debian/Ubuntu上：
     ```bash
     sudo iptables-save > /etc/iptables/rules.v4
     ```
   - 在CentOS/RHEL上：
     ```bash
     sudo service iptables save
     ```

### `firewalld` 防火墙配置

`firewalld` 是一个动态管理防火墙的工具，提供基于区域的规则集管理。它取代了`iptables`，并简化了防火墙规则的管理。

#### 基本用法

`firewalld`使用区域（zone）来定义不同网络接口的安全级别。常见的区域有`public`、`work`、`home`、`dmz`等。

#### 常见命令

1. **启动、停止和重启`firewalld`服务**：
   ```bash
   sudo systemctl start firewalld
   sudo systemctl stop firewalld
   sudo systemctl restart firewalld
   ```

2. **查看防火墙状态**：
   ```bash
   sudo firewall-cmd --state
   ```

3. **查看活动的区域**：
   ```bash
   sudo firewall-cmd --get-active-zones
   ```

4. **添加规则**（如允许SSH的22端口）：
   ```bash
   sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
   sudo firewall-cmd --reload
   ```

5. **删除规则**：
   ```bash
   sudo firewall-cmd --zone=public --remove-port=22/tcp --permanent
   sudo firewall-cmd --reload
   ```

6. **允许特定服务**：
   ```bash
   sudo firewall-cmd --zone=public --add-service=http --permanent
   sudo firewall-cmd --reload
   ```



### 示例操作

以下是一些实际操作示例，以帮助您更好地理解：

#### 使用基本的网络命令

1. 使用`ifconfig`显示网络接口信息：
   ```bash
   ifconfig
   ```

2. 使用`ip`命令显示所有网络接口的信息：
   ```bash
   ip addr show
   ```

3. 使用`ping`测试与`google.com`的连接：
   ```bash
   ping google.com
   ```

4. 使用`traceroute`显示到`google.com`的路径：
   ```bash
   traceroute google.com
   ```

5. 使用`netstat`显示所有活动的网络连接：
   ```bash
   netstat -a
   ```

6. 使用`ss`显示所有TCP连接：
   ```bash
   ss -t
   ```

#### 使用 SSH

1. 连接到远程服务器：
   ```bash
   ssh user@remote_host
   ```

2. 生成SSH密钥对：
   ```bash
   ssh-keygen -t rsa -b 4096
   ```

3. 将公钥复制到远程服务器：
   ```bash
   ssh-copy-id user@remote_host
   ```

4. 创建一个本地到远程的SSH隧道：
   ```bash
   ssh -L 8080:remote_host:80 user@remote_host
   ```

#### 使用`iptables`配置防火墙

1. **查看当前规则**：
   ```bash
   sudo iptables -L
   ```

2. **允许SSH流量**：
   ```bash
   sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
   ```

3. **阻止特定IP地址**：
   ```bash
   sudo iptables -A INPUT -s 192.168.1.100 -j DROP
   ```

4. **保存规则**（Debian/Ubuntu）：
   ```bash
   sudo iptables-save > /etc/iptables/rules.v4
   ```

5. **删除规则**：
   ```bash
   sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT
   ```

#### 使用`firewalld`配置防火墙

1. **启动`firewalld`服务**：
   ```bash
   sudo systemctl start firewalld
   ```

2. **查看活动的区域**：
   ```bash
   sudo firewall-cmd --get-active-zones
   ```

3. **允许SSH流量**：
   ```bash
   sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
   sudo firewall-cmd --reload
   ```

4. **允许HTTP服务**：
   ```bash
   sudo firewall-cmd --zone=public --add-service=http --permanent
   sudo firewall-cmd --reload
   ```

5. **删除规则**：
   ```bash
   sudo firewall-cmd --zone=public --remove-port=22/tcp --permanent
   sudo firewall-cmd --reload
   ```