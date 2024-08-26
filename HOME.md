

### 1. 域名
   - 购买域名
     - 域名注册商（如 GoDaddy、Namecheap）
   - DNS 配置
     - 将域名指向服务器 IP 地址

### 2. 服务器设置
   - 获取服务器
     - 云服务提供商（如 AWS、DigitalOcean、Linode）
     - 物理服务器
   - 操作系统安装
     - CentOS 操作系统

### 3. 安装 Web 服务器
   - 选择 Web 服务器
     - Apache
       - 安装 Apache
         - `sudo yum install httpd`
         - `sudo systemctl start httpd`
         - `sudo systemctl enable httpd`
     - Nginx
       - 安装 Nginx
         - `sudo yum install epel-release`
         - `sudo yum install nginx`
         - `sudo systemctl start nginx`
         - `sudo systemctl enable nginx`

### 4. 配置虚拟主机
   - Apache
     - 创建虚拟主机配置文件
       - `/etc/httpd/conf.d/example.com.conf`
     - 创建站点目录
       - `sudo mkdir -p /var/www/html/example.com`
       - `sudo chown -R apache:apache /var/www/html/example.com`
     - 重启 Apache
       - `sudo systemctl restart httpd`
   - Nginx
     - 创建虚拟主机配置文件
       - `/etc/nginx/conf.d/example.com.conf`
     - 创建站点目录
       - `sudo mkdir -p /usr/share/nginx/html/example.com`
       - `sudo chown -R nginx:nginx /usr/share/nginx/html/example.com`
     - 重启 Nginx
       - `sudo systemctl restart nginx`

### 5. 配置 SSL 证书
   - 获取 SSL 证书
     - Let's Encrypt
     - 其他证书颁发机构（CA）
   - 安装 Certbot
     - `sudo yum install epel-release`
     - Certbot 安装命令
       - `sudo yum install certbot python2-certbot-apache` （对于 Apache）
       - `sudo yum install certbot python2-certbot-nginx` （对于 Nginx）
   - 获取证书并自动配置
     - `sudo certbot --apache`
     - `sudo certbot --nginx`
     - 按提示输入域名并完成配置

### 6. 配置防火墙
   - 打开 HTTP 和 HTTPS 端口
     - `sudo firewall-cmd --permanent --add-service=http`
     - `sudo firewall-cmd --permanent --add-service=https`
     - `sudo firewall-cmd --reload`

### 7. 部署网站内容
   - 上传网站内容到根目录
     - `/var/www/html/example.com` （Apache）
     - `/usr/share/nginx/html/example.com` （Nginx）

### 8. 测试网站
   - 在浏览器中访问域名
     - `http://example.com`
     - `https://example.com`
