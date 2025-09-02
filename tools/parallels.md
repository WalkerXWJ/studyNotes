# patallels虚拟机文件

通过burp拦截流量获取到的，可以通过wget等工具直接下载软件包，解决patallels下载巨慢的问题。
```bash
# 下面的文件从链接中获取的 https://download.parallels.com/desktop/v20/appliances_arm_pro.xml
https://download.parallels.com/desktop/appliances/Ubuntu_24.04_arm.tar.gz
https://download.parallels.com/desktop/appliances/Ubuntu_24.04_rosetta_arm.tar.gz
https://download.parallels.com/desktop/appliances/Fedora_40_arm.tar.gz
https://download.parallels.com/desktop/appliances/Debian_GNU_12.6_arm.tar.gz
https://download.parallels.com/desktop/appliances/Kali_2024.2_arm.tar.gz
```
# kali更换国内源-中科大
```bash
# 备份源文件
mv /etc/apt/souces.list /etc/apt/sources.list.bak
# 编辑源文件
vim /etc/apt/souces.list
# 添加如下内容
deb http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free non-free-firmware
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free non-free-firmware
# 更新资源列表 和 升级已安装的软件
apt update && apt upgrade -y
# 更新升级整个kali系统，会自动处理出现的软件冲突
apt dist-upgrade
# 清理安装文件
apt clean
```
