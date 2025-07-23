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
apt install maven
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