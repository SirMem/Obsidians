---
Project: "[[计算机网络]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-11-06
Connected:
---

#review

## 监听地址LAN
是的，在 Clash 中，启用 **Open LAN** 功能（即将 `allow-lan` 设为 `true`）就相当于将监听地址设置为 `0.0.0.0`，这样其他设备（如局域网内的设备或虚拟机）就可以通过宿主机的 IP 地址和 Clash 的代理端口访问代理服务。

### 具体说明：
- **allow-lan: true**：允许局域网内其他设备访问 Clash 代理。启用后，Clash 会在 `0.0.0.0` 上监听代理端口。
- **bind-address**（可选）：可以进一步指定监听的 IP 地址。默认是 `0.0.0.0`，即所有网络接口。如果只希望特定的网卡（例如 Wi-Fi 接口）上开放，可以指定具体的 IP 地址。

### 示例配置
在 `config.yaml` 文件中，相关配置通常如下：
```yaml
allow-lan: true
bind-address: 0.0.0.0  # 可选项，默认已是 0.0.0.0
port: 7890  # 代理端口
```

启用后，你可以在虚拟机或其他设备上通过宿主机的 IP 地址（如 `192.168.43.154:7890`）访问代理服务。

## 虚拟机
可以的，宿主机和虚拟机可以同时使用代理工具。只要虚拟机通过自己的代理工具来访问网络，就不会受到宿主机代理配置的影响。这种配置在以下场景中都适用：

1. **宿主机和虚拟机各自使用独立的代理工具**：虚拟机在其系统内单独设置代理，指向不同的服务器或端口。

2. **虚拟机通过宿主机的代理访问网络**：虚拟机直接配置成使用宿主机的代理端口（例如，通过宿主机的 IP 地址和代理端口号），这样虚拟机的所有流量会通过宿主机的代理。

3. **宿主机和虚拟机代理级联**：如果虚拟机中的代理工具指向宿主机的代理，宿主机再通过外部代理进行转发，这样可以形成多层代理链路。

### 注意事项
- **端口冲突**：确保宿主机和虚拟机的代理工具不使用相同的端口，尤其是在桥接模式下，虚拟机会被分配到一个独立的 IP 地址，通常不会发生端口冲突。
- **代理工具设置**：如果虚拟机通过宿主机代理访问外部网络，虚拟机中的代理设置应指向宿主机的 IP 和代理端口。

通过这种方式，宿主机和虚拟机可以灵活设置各自的代理，不会相互干扰。


## Linux网络终端配置
要在 Linux 终端中通过代理进行网络传输，可以有几种方式，具体取决于代理的类型（如 HTTP 代理、SOCKS 代理）以及是否需要全局代理。以下是一些常用的设置方法：

### 1. 配置环境变量（HTTP/HTTPS 代理）

许多 Linux 应用程序（如 `curl`、`wget` 等）会自动使用系统的环境变量配置代理。这种方法适用于 HTTP 和 HTTPS 代理。

在终端中设置代理环境变量：

```bash
export http_proxy="http://proxy-server:port"
export https_proxy="http://proxy-server:port"
export ftp_proxy="http://proxy-server:port"
export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"  # 设置不使用代理的地址
```

- 将 `proxy-server` 替换为代理服务器的 IP 地址或域名，`port` 替换为代理端口。
- `no_proxy` 环境变量用于指定不需要代理的地址。

要永久应用此配置，可以将这些 `export` 语句添加到 `~/.bashrc` 或 `~/.bash_profile` 文件中。

### 2. 配置全局代理（SOCKS 代理）

对于 SOCKS 代理，可以使用 `proxychains` 或 `tsocks` 工具，将所有网络流量重定向到 SOCKS 代理。

#### 使用 `proxychains`

1. 安装 `proxychains`：

   ```bash
   sudo apt install proxychains  # Ubuntu/Debian
   sudo yum install proxychains  # CentOS/RHEL
   ```

2. 编辑 `proxychains` 的配置文件（通常在 `/etc/proxychains.conf`），并添加 SOCKS 代理地址：

   ```bash
   socks5 127.0.0.1 1080  # 替换为你的 SOCKS 代理的 IP 和端口
   ```

3. 使用 `proxychains` 启动命令，例如：

   ```bash
   proxychains curl http://example.com
   ```

   这样，所有通过 `proxychains` 的流量都会经过 SOCKS 代理。

#### 使用 `tsocks`

`tsocks` 是另一个可以将流量重定向到 SOCKS 代理的工具，配置步骤与 `proxychains` 类似。

1. 安装 `tsocks`：

   ```bash
   sudo apt install tsocks  # Ubuntu/Debian
   sudo yum install tsocks  # CentOS/RHEL
   ```

2. 配置 `/etc/tsocks.conf`，添加 SOCKS 代理信息：

   ```conf
   server = 127.0.0.1
   server_port = 1080
   ```

3. 使用 `tsocks` 启动命令，例如：

   ```bash
   tsocks curl http://example.com
   ```

### 3. 使用 `iptables` 或 `nftables` 设置透明代理（高级）

如果你想将所有流量（包括非终端应用）强制通过代理，可以使用 `iptables` 或 `nftables` 配置透明代理。这通常需要使用一个支持透明代理的代理服务器（如 Squid 或 Shadowsocks），并在服务器上启用端口转发。

```bash
# 将所有流量转发到代理服务器的端口（假设代理在 1234 端口）
sudo iptables -t nat -A OUTPUT -p tcp --dport 80 -j REDIRECT --to-port 1234
sudo iptables -t nat -A OUTPUT -p tcp --dport 443 -j REDIRECT --to-port 1234
```

这种方法比较复杂，且需要管理员权限。配置不当可能导致网络问题，所以通常在需要透明代理的服务器上使用。

### 4. 使用代理命令启动应用（SOCKS 代理）

有些应用支持直接通过代理运行，例如 `curl` 和 `wget`。如果你的代理是 SOCKS5，可以使用如下方式：

```bash
curl --socks5 127.0.0.1:1080 http://example.com
wget -e use_proxy=yes -e http_proxy=socks5://127.0.0.1:1080 http://example.com
```

### 5. 通过 VPN 配置代理（全局代理）

使用 VPN 是一种全局代理的替代方案。VPN 会将所有流量通过隧道转发到远程服务器，常见的 VPN 工具有 OpenVPN 和 WireGuard。配置 VPN 后，终端的网络流量将通过 VPN 隧道，达到全局代理的效果。

---

这些方法适用于不同的场景，选择适合的代理方法即可。如果有多个工具需要代理，可以通过设置环境变量或使用 `proxychains` 这样的工具来简化代理配置。

sudo systemctl restart networking

## 虚拟机网络配置
在 VirtualBox 中配置网络，可以通过多种方式设置虚拟机的网络连接方式，具体取决于你的需求。以下是几种常见的网络配置方式及其步骤：

### 1. **NAT（网络地址转换）**
   - **适用场景**：适用于虚拟机访问外部网络（如互联网），但外部网络无法直接访问虚拟机。
   - **配置步骤**：
     1. 选择虚拟机，点击 `设置`。
     2. 在 `网络` 选项卡下，确保启用 `适配器 1`。
     3. 在 `连接方式` 下拉菜单中选择 `NAT`。
     4. 点击 `确定` 保存配置。
     5. 启动虚拟机后，虚拟机将能够访问互联网，但无法直接从外部访问。

### 2. **桥接网络（Bridged Adapter）**
   - **适用场景**：虚拟机与物理主机在同一局域网中，虚拟机可以像物理主机一样直接连接到外部网络。
   - **配置步骤**：
     1. 选择虚拟机，点击 `设置`。
     2. 在 `网络` 选项卡下，启用 `适配器 1`。
     3. 在 `连接方式` 下拉菜单中选择 `桥接网络`。
     4. 在 `名称` 下拉菜单中选择你主机的网络接口（如 Wi-Fi 或有线网卡）。
     5. 点击 `确定` 保存配置。
     6. 启动虚拟机，虚拟机将获得与主机相同的局域网 IP 地址，可以直接访问外部网络，并且可以被外部设备访问。

### 3. **仅主机网络（Host-Only Adapter）**
   - **适用场景**：虚拟机与物理主机之间可以通信，但虚拟机不能访问外部网络。
   - **配置步骤**：
     1. 选择虚拟机，点击 `设置`。
     2. 在 `网络` 选项卡下，启用 `适配器 1`。
     3. 在 `连接方式` 下拉菜单中选择 `仅主机网络`。
     4. 在 `名称` 下拉菜单中选择一个已创建的 `Host-Only Adapter`（如果没有，可以在 VirtualBox 主界面中创建一个新的 Host-Only 网络）。
     5. 点击 `确定` 保存配置。
     6. 启动虚拟机，虚拟机和主机之间可以通信，但不能访问外部网络。

### 4. **内部网络（Internal Network）**
   - **适用场景**：多个虚拟机在一个内部网络中可以互相通信，但它们无法访问外部网络，也不会被外部设备访问。
   - **配置步骤**：
     1. 选择虚拟机，点击 `设置`。
     2. 在 `网络` 选项卡下，启用 `适配器 1`。
     3. 在 `连接方式` 下拉菜单中选择 `内部网络`。
     4. 在 `名称` 中输入一个自定义的网络名称，确保其他虚拟机使用相同的网络名称才能相互通信。
     5. 点击 `确定` 保存配置。
     6. 启动虚拟机，虚拟机将与其他同样配置的虚拟机进行通信，但无法访问外部网络。

### 5. **端口转发（Port Forwarding）**
   - **适用场景**：当使用 NAT 网络时，无法直接访问虚拟机，但可以通过端口转发将外部请求转发到虚拟机。
   - **配置步骤**：
     1. 选择虚拟机，点击 `设置`。
     2. 在 `网络` 选项卡下，确保选择了 `NAT` 网络模式。
     3. 点击右侧的 `高级` 按钮，选择 `端口转发`。
     4. 在弹出的窗口中，点击 `添加规则`。
     5. 设置需要转发的端口（如将宿主机的端口 8080 转发到虚拟机的 80 端口）。
     6. 点击 `确定` 保存设置。
     7. 启动虚拟机，通过宿主机的指定端口可以访问虚拟机。

### 6. **自定义虚拟网络**
   - **适用场景**：为不同虚拟机创建独立的自定义网络进行更灵活的网络配置。
   - **配置步骤**：
     1. 打开 VirtualBox 主界面，点击 `文件` -> `主机网络管理器`。
     2. 在管理器中创建一个新的虚拟网络，并为其指定 IP 范围。
     3. 在虚拟机的 `网络` 配置中，选择该自定义网络作为适配器的连接方式。
     4. 配置虚拟机后，启用虚拟机，它将与同一自定义网络中的其他虚拟机进行通信。

每种网络模式都适用于不同的场景，根据你的需求选择合适的网络配置。如果你有具体的配置需求，可以告诉我，我会帮你进一步优化配置方案。