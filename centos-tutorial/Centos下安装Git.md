# Centos下安装Git

### 1. 安装依赖

```bash
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```

### 2. 编译安装git

#### 2.1 下载源码包

```bash
wget https://www.kernel.org/pub/software/scm/git/git-2.8.3.tar.gz
```

#### 2.2 解压

```bash
tar -zxvf git-2.8.3.tar.gz
```

#### 2.3 编译安装

```bash
进入到安装包目录
cd git-2.8.3  
配置安装目录  
./configure prefix=/usr/local/git/   
编译安装
make && make install
```

### 3. 配置git指令到PATH中

#### 3.1 修改环境变量

```bash
环境变量文件
vi /etc/profile   
最后一行添加上一下命令，根据安装目录而定
export PATH=$PATH:/usr/local/git/bin
```

#### 3.2 使配置生效

```bash
source /etc/profile
```

### 4. 查看版本

```bash
git --version
显示如下
git version 2.8.3
```
