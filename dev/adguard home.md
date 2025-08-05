项目地址：[https://github.com/AdguardTeam/AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)
环境：ubuntu 24
### 安装
```bash
curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v
```
### 使用
```bash
sudo /opt/AdGuardHome/AdGuardHome -s start|stop|restart|status|install|uninstall
```
### 配置访问地址
```http
http://host-ip:3000
```
### 解决ubuntu安装问题：
错误：`validating ports: listen tcp 0.0.0.0:53: bind: address already in use`
原因：ubuntu有一个本地dns，使用53端口，阻止了adguard home绑定到127.0.0.1:53
```bash
# validating ports: listen tcp 0.0.0.0:53: bind: address already in use
# 查看占空53端口的服务
sudo lsof -i :53
COMMAND   PID            USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
systemd-r 707 systemd-resolve   14u  IPv4   8481      0t0  UDP _localdnsstub:domain 
systemd-r 707 systemd-resolve   15u  IPv4   8482      0t0  TCP _localdnsstub:domain (LISTEN)
systemd-r 707 systemd-resolve   16u  IPv4   8483      0t0  UDP _localdnsproxy:domain 
systemd-r 707 systemd-resolve   17u  IPv4   8484      0t0  TCP _localdnsproxy:domain (LISTEN)
```
解决问题：
```bash
# 创建目录
sudo mkdir -p /etc/systemd/resolved.conf.d 
# 进入目录
cd /etc/systemd/resolved.conf.d
# vim 编辑创建文件
vim adguardhome.conf
```
向 adguardhome.conf 粘贴如下内容，并保持退出。
```bash
[Resolve]
DNS=127.0.0.1
DNSStubListener=no
```
激活文件`resolv.conf`
```bash
sudo mv /etc/resolv.conf /etc/resolv.conf.backup 
sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
# 如果出现如下问题
# root@ubuntu24:/etc/systemd/resolved.conf.d# sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
# sudo: unable to resolve host ubuntu24: Temporary failure in name resolution
# 解决办法如下：
# 编辑 /etc/hosts
# 添加如下内容
# 127.0.0.1   localhost
# 127.0.1.1   ubuntu24
```
重启DNSStubListener
```bash
sudo systemctl reload-or-restart systemd-resolved
```
### 设置dns
访问系统web页面，登录后，设置-dns设置
```bash
# 注释掉 https://dns10.quad9.net/dns-query 因国内不可用
# 增加如下的公网dns 公司使用则配置公司的dns
8.8.8.8
8.8.4.4
```
下滑页面，点击 测试上游dns，显示【指定的 DNS 服务器现已正常运行】则配置没问题。
### 配置证书已支持加密dns协议
证书生成脚本
```bash
#!/bin/bash

# 获取当前日期时间作为文件名前缀
TIMESTAMP=$(date +"%Y%m%d%H%M%S")
PUBLIC_KEY_FILE="$HOME/Downloads/${TIMESTAMP}_publickey.pem"
PRIVATE_KEY_FILE="$HOME/Downloads/${TIMESTAMP}_privatekey.pem"

# 生成自签名证书和私钥（有效期10年）
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
  -keyout "$PRIVATE_KEY_FILE" \
  -out "$PUBLIC_KEY_FILE" \
  -subj "/C=CN/ST=Beijing/L=Beijing/O=My Company/CN=localhost"

# 检查文件是否生成成功
if [ -f "$PUBLIC_KEY_FILE" ] && [ -f "$PRIVATE_KEY_FILE" ]; then
  echo "证书生成成功:"
  echo "公钥文件: $PUBLIC_KEY_FILE"
  echo "私钥文件: $PRIVATE_KEY_FILE"
  
  # 输出文件内容到控制台
  echo -e "\n公钥内容:"
  cat "$PUBLIC_KEY_FILE"
  
  echo -e "\n私钥内容:"
  cat "$PRIVATE_KEY_FILE"
else
  echo "证书生成失败!"
  exit 1
fi
```
生成证书后
1. 点击【设置】-【加密设置】
2. 勾选【启用加密】
3. 下滑找到【证书】，选择【粘贴证书内容】，将公钥复制粘贴进去，提示- 证书链无效，忽略即可。
4. 下滑找到【私钥】，选择【粘贴证书内容】，将公钥复制粘贴进去，点击【保存配置】。
### 给使用dns的设备安装证书
mac电脑
1. 打开访达，进入下载目录
2. 双击publickey.pem文件，输入密码或指纹，选择【系统】，再次输入密码或指纹。
3. 在钥匙访问窗口，点击【系统】，找到localhost证书，双击后选择【信任】，选择使用此证书时【始终信任】，关闭窗口，输入密码或指纹。
4. 打开系统设置，【网络】-【详细信息】-【DNS】，选择dns内容，按减号删除原有dns，添加你的adguard home服务的ip地址即可。到此配置结束，可以正常使用了。
### 添加更多的拦截规则
访问adguard home的问题页面，【过滤器】-【DNS黑名单】-【添加黑名单】-【从列表中选择】，喜欢什么就添加点什么。