# wsl2安装ubuntu

> windows使用wsl2实现ubuntu子系统

## 下载

先去windows store下载ubuntu 20.04并安装

```bash
# 查看当前wsl现有linux版本
wsl -l --all -v
# wsl关机
wsl --shutdown
# 导出文件到其他地方
wsl --export Ubuntu-20.04 d:\wsl-ubuntu20.04.tar
# 注销当前版本
wsl --unregister Ubuntu-20.04
# 导入镜像到wsl
wsl --import Ubuntu-20.04 d:\wsl-ubuntu20.04 d:\wsl-ubuntu20.04.tar --version 2
# 注册默认用户
ubuntu2004 config --default-user daichi
# 删除之前导出的文件
del d:\wsl-ubuntu20.04.tar
```

## 设置root用户

新装的ubuntu中root用户没有设置密码，设置root用户密码

```bash
sudo passwd root
```

## 安装docker

安装docker

```bash
# 清除系统中内置旧版docker
sudo apt-get remove docker docker-engine docker.io containerd runc

# 更新apt
sudo apt-get update

# 安装 apt 依赖包，用于通过HTTPS来获取仓库:
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# 添加 Docker 的官方 GPG 密钥
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

# 通过搜索指纹的后8个字符，验证您现在是否拥有带有指纹的密钥
sudo apt-key fingerprint 0EBFCD88

# 设置稳定版仓库
sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
  $(lsb_release -cs) \
  stable"

#更新 apt 包索引
sudo apt-get update

# 安装最新版本的 Docker Engine-Community 和 containerd ，或者转到下一步安装特定版本：
sudo apt-get install docker-ce docker-ce-cli containerd.io

# 要安装特定版本的 Docker Engine-Community，请在仓库中列出可用版本，然后选择一种安装。列出您的仓库中可用的版本
apt-cache madison docker-ce

# 安装特定版本
sudo apt-get install docker-ce=5:20.10.12~3-0~ubuntu-focal docker-ce-cli=5:20.10.12~3-0~ubuntu-focal containerd.io
```

## 安装docker镜像

启动docker安装开发环境

```bash
sudo service docker start
# docker安装redis
docker pull redis:latest
docker run -itd --name redis-linux -p 6379:6379 redis

# docker安装oracle
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
docker run -d -p 1521:1521 --name oracle11g registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
docker exec -it oracle11g bash

# 进入docker容器root
su root
# 打开配置文件
vi /etc/profile

# 最下面保存三行语句
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export ORACLE_SID=helowin
export PATH=$ORACLE_HOME/bin:$PATH
# 建立软链接
ln -s $ORACLE_HOME/bin/sqlplus /usr/bin

# 回到oracle用户，不要忘记-
su - oracle

# 打开sqlplus
sqlplus /nolog
# 以sysdba连接
conn /as sysdba

# 更改system,sys密码
alter user system identified by system;
alter user sys identified by sys;
# 设置密码永不过期
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```
