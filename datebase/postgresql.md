[主页](./../README.md) 

# PostgreSQL

官方网址：https://www.postgresql.org/

postgresql：世界上最先进的开源关系数据库ORDBMS （​Object-Relational Database Management System​​，对象关系数据库管理系统），由加州大学伯克利分校计算机科学系开发（University of California at Berkeley Computer Science Department.）。

官方文档：[PostgreSQL: Documentation](https://www.postgresql.org/docs/)

官方安装和使用说明：[PostgreSQL: Downloads](https://www.postgresql.org/download/)

## 1. macOS Sequoia使用PostgreSQL

### 1.1. 安装

```bash
brew install postgresql@17
```

```bash
# 添加环境path
echo 'export PATH="/opt/homebrew/opt/postgresql@17/bin:$PATH"' >> ~/.zshrc
# 添加添加编译器环境
export LDFLAGS="-L/opt/homebrew/opt/postgresql@17/lib"
export CPPFLAGS="-I/opt/homebrew/opt/postgresql@17/include"
# 设置开启时启动postgresql    未执行
brew services start postgresql@17
# 设置禁用后台服务    未执行
LC_ALL="C" /opt/homebrew/opt/postgresql@17/bin/postgres -D /opt/homebrew/var/postgresql@17
# brew 卸载数据库后清理链接符号
 brew cleanup --prune-prefix
```

### 1.2. 基础使用

- 公约

管理员：负责安装和运营服务器的人。

用户：正在使用或想要使用postgresql的系统的任何部分的人。

```bash
# 创建默认的数据库集群
initdb --locale=C -E UTF-8 /opt/homebrew/var/postgresql@17
# 
```

### 1.3. 在项目中使用
