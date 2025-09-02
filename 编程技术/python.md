# python 官网文档学习
## [venv虚拟环境创建](https://docs.python.org/zh-cn/3.13/library/venv.html#module-venv)
venv是创建虚拟环境的标准工具，从python 3.3 开始成为python的组成部分，从python 3.4 开始venv会默认安装pip到所有创建的全部虚拟环境。

# book-python黑帽子
env：kali linux
## python环境
### 更新python
更新python到最新的python3
```bash
apt upgrade python3
```
### 创建虚拟环境

虚拟环境：
其实就是一个文件夹📁，里面存放了完整的python软件包和安装的第三方包。可以把不同需求的软件包隔离开来，每个环境都有自己的一套模块和依赖关系，不会干扰其他项目的依赖管理。

安装python3-venv
```bash
apt install python-venv
```
