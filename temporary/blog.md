# Blog

## 1. OneBlog

### 1.1. 安装OneBlog
下载和打包项目
```bash
# clone oneBlog
git clone https://github.com/zhangyd-c/OneBlog
# install docker-compose
apt install docker-compose -y
# install maven
apt install maven -y 
# cd dir
cd OneBlog
# 打包项目
mvn clean package -Dmaven.test.skip=true -Pdev 

```
配置docker环境

```bash
# 进入配置文件的目录
cd ./docs/docker
# 修复.env文件 打开文件就知道要修改什么了
vim .env
# 启动项目
docker-compose -p oneblog up -d
```

验证服务
```bash
# 查看容器状态
docker-compose -p oneblog ps

# 查看日志
docker-compose -p oneblog logs -f
```
解决报错 [参考链接](https://cloud.tencent.com/developer/article/2516747)
```bash
root@ubuntu24:~/OneBlog/docs/docker# docker-compose -p oneblog up -d
Pulling blog-redis (redis:)...
ERROR: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
```
脚本
```bash
#!/bin/bash

# Docker 镜像修复脚本
# 功能：自动解决 "docker: Get https://registry-1.docker.io/v2/: net/http: request canceled" 错误

# 颜色定义
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[0;33m'
NC='\033[0m' # No Color

# 检查是否以root运行
if [ "$(id -u)" -ne 0 ]; then
    echo -e "${RED}错误: 此脚本需要以root权限运行${NC}"
    exit 1
fi

# 检查Docker是否安装
if ! command -v docker &> /dev/null; then
    echo -e "${RED}错误: Docker未安装，请先安装Docker${NC}"
    exit 1
fi

# 定义镜像源列表
MIRRORS=(
    "https://docker.registry.cyou"
    "https://docker-cf.registry.cyou"
    "https://dockercf.jsdelivr.fyi"
    "https://docker.jsdelivr.fyi"
    "https://mirror.aliyuncs.com"
    "https://dockerproxy.com"
    "https://mirror.baidubce.com"
    "https://docker.m.daocloud.io"
    "https://docker.nju.edu.cn"
    "https://docker.mirrors.sjtug.sjtu.edu.cn"
    "https://docker.mirrors.ustc.edu.cn"
    "https://mirror.iscas.ac.cn"
    "https://registry.docker-cn.com"
    "http://hub-mirror.c.163.com"
    "https://mirrors.tuna.tsinghua.edu.cn/"
)

# 备份现有配置文件
backup_config() {
    if [ -f /etc/docker/daemon.json ]; then
        cp /etc/docker/daemon.json /etc/docker/daemon.json.bak
        echo -e "${YELLOW}已备份现有配置文件到 /etc/docker/daemon.json.bak${NC}"
    fi
}

# 配置Docker镜像源
configure_mirrors() {
    echo -e "${GREEN}正在配置Docker镜像源...${NC}"
    
    # 创建配置JSON
    CONFIG_JSON='{
  "registry-mirrors": ['
    
    # 添加镜像源
    for mirror in "${MIRRORS[@]}"; do
        CONFIG_JSON+="\n    \"$mirror\","
    done
    
    # 移除最后一个逗号
    CONFIG_JSON=${CONFIG_JSON%,}
    
    CONFIG_JSON+='
  ],
  "insecure-registries": [
    "docker.mirrors.ustc.edu.cn"
  ],
  "debug": true,
  "experimental": false
}'
    
    # 写入配置文件
    echo -e "$CONFIG_JSON" > /etc/docker/daemon.json
    echo -e "${GREEN}Docker镜像源配置完成${NC}"
}

# 重启Docker服务
restart_docker() {
    echo -e "${YELLOW}正在重启Docker服务...${NC}"
    systemctl restart docker
    sleep 3
    if systemctl is-active --quiet docker; then
        echo -e "${GREEN}Docker服务重启成功${NC}"
    else
        echo -e "${RED}Docker服务重启失败${NC}"
        exit 1
    fi
}

# 测试网络连接
test_connection() {
    echo -e "${GREEN}测试Docker Hub连接性...${NC}"
    if curl -sSf https://registry-1.docker.io/v2/ > /dev/null; then
        echo -e "${GREEN}连接Docker Hub成功${NC}"
    else
        echo -e "${RED}连接Docker Hub失败${NC}"
        echo -e "${YELLOW}建议检查网络设置、防火墙或代理配置${NC}"
    fi
}

# 检查系统资源
check_resources() {
    echo -e "\n${GREEN}检查系统资源...${NC}"
    
    # 内存检查
    echo -e "${YELLOW}内存使用情况:${NC}"
    free -h
    
    # 磁盘检查
    echo -e "\n${YELLOW}磁盘使用情况:${NC}"
    df -h
    
    # Docker存储检查
    echo -e "\n${YELLOW}Docker存储使用情况:${NC}"
    docker system df
}

# 查看Docker日志
view_logs() {
    echo -e "\n${GREEN}查看Docker日志(最后20行)...${NC}"
    journalctl -u docker.service -n 20 --no-pager
}

# 测试Docker拉取
test_pull() {
    echo -e "\n${GREEN}测试拉取hello-world镜像...${NC}"
    if docker pull hello-world; then
        echo -e "${GREEN}镜像拉取成功！问题已解决${NC}"
    else
        echo -e "${RED}镜像拉取仍然失败${NC}"
        echo -e "${YELLOW}请检查以下可能原因："
        echo "1. 网络连接问题"
        echo "2. 防火墙设置"
        echo "3. 代理配置"
        echo "4. 系统资源不足"
        echo "5. Docker服务异常${NC}"
    fi
}

# 主函数
main() {
    echo -e "\n${GREEN}=== Docker镜像拉取问题修复脚本 ===${NC}"
    
    backup_config
    configure_mirrors
    restart_docker
    test_connection
    check_resources
    view_logs
    test_pull
    
    echo -e "\n${GREEN}=== 修复步骤完成 ===${NC}"
    echo -e "${YELLOW}如果问题仍然存在，请检查网络环境或联系系统管理员${NC}"
}

# 执行主函数
main
```
报错
```bash
构建博客-mysql  
[ ] 构建 242.7s （2/2） 完成 docker：default  
=> [内部] 从 Dockerfile 0.0s 加载构建定义  
=> =>传输 dockerfile：350B 0.0s  
=> WARN：MaintainerDeprecated：维护者指令已被弃用，转而使用标签（第 3 行）0.0s  
=> 错误 [内部] 加载 docker.io/library/mysql:5.7 242.6s 的元数据  
------  
> [内部] 加载 docker.io/library/mysql:5.7 的元数据：  
------  
  
发现 1 个警告（使用 docker --debug 展开）：  
- MaintainerDeprecated：维护者指令已被弃用，转而使用标签（第 3 行）  
Dockerfile：1  
--------------------  
1 |>>>来自 mysql：5.7  
2 |  
3 |维护者 yadong.zhang0415@gmail.com  
--------------------  
错误：构建失败：解决失败：mysql：5.7：无法解析 docker.io/library/mysql:5.7 的源元数据：清单中与平台不匹配：未找到  
错误：服务“blog-mysql”构建失败：构建失败
```