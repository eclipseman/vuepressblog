---
title: Zero SSL证书自动续期
createTime: 2025/03/13 09:51:09
permalink: /article/6nnggszz/
tags:
  - tech
  - ssl
  - markdown
  - acme.sh
  - 腾讯云
---


## 使用 acme.sh 申请和安装 SSL 证书

## 环境准备

### 安装 acme.sh

```bash
curl https://get.acme.sh | sh -s email=eclipseman@163.com
```

### 配置 shell alias

编辑 `~/.bashrc` 文件，添加以下内容：

```bash
alias acme.sh=~/.acme.sh/acme.sh
```

保存并退出后，执行以下命令使配置生效：

```bash
source ~/.bashrc
```

### 设置腾讯云 API 密钥

编辑 `/etc/profile` 文件，添加以下内容：

```bash
export Tencent_SecretId="腾讯云SecretId"
export Tencent_SecretKey="腾讯云SecretKey"
```

保存并退出后，执行以下命令使配置生效：

```bash
source /etc/profile
```

### 选择证书颁发机构并关联邮箱

```bash
acme.sh --set-default-ca --server zerossl
acme.sh --register-account -m eclipseman@163.com --server zerossl
```

## 通用流程

### 申请证书

```bash
acme.sh --dns dns_tencent --issue -d *.域名 -d 域名
```

### 安装证书

```bash
acme.sh --installcert -d *.域名 \
        --key-file /证书存储路径/域名.key \
        --fullchain-file /证书存储路径/fullchain.cer \
        --reloadcmd "/usr/sbin/nginx -s reload"
```

### 查看证书文件

```bash
ls /证书存储路径
```

### 配置 Nginx

在 Nginx 配置文件中，设置以下参数：

```nginx
ssl_certificate  /证书存储路径/fullchain.cer;
ssl_certificate_key /证书存储路径/域名.key;
```

## 具体案例

### ndlink.cn

#### 创建证书存储目录

```bash
mkdir -p /home/ssl/ndlink.cn
```

#### 申请证书

```bash
acme.sh --dns dns_tencent --issue -d *.ndlink.cn -d ndlink.cn
```

#### 安装证书

```bash
acme.sh --installcert -d *.ndlink.cn \
        --key-file /home/ssl/ndlink.cn/*.ndlink.cn.key \
        --fullchain-file /home/ssl/ndlink.cn/fullchain.cer \
        --reloadcmd "/usr/sbin/nginx -s reload"
```

### tpr.ndlink.cn

#### 创建证书存储目录

```bash
mkdir -p /home/ssl/tpr.ndlink.cn
```

#### 申请证书

```bash
acme.sh --dns dns_tencent --issue -d *.tpr.ndlink.cn -d tpr.ndlink.cn
```

#### 安装证书

```bash
acme.sh --installcert -d *.tpr.ndlink.cn \
        --key-file /home/ssl/tpr.ndlink.cn/*.tpr.ndlink.cn.key \
        --fullchain-file /home/ssl/tpr.ndlink.cn/fullchain.cer \
        --reloadcmd "/usr/sbin/nginx -s reload"
```

### iacove.com

#### 创建证书存储目录

```bash
mkdir -p /home/iacove/ssl/iacove.com
```

#### 申请证书

```bash
acme.sh --dns dns_tencent --issue -d *.iacove.com -d iacove.com
```

#### 安装证书

```bash
acme.sh --installcert -d *.iacove.com \
        --key-file /home/iacove/ssl/iacove.com/*.iacove.com.key \
        --fullchain-file /home/iacove/ssl/iacove.com/fullchain.cer \
        --reloadcmd "/usr/sbin/nginx -s reload"
```

### cnbma.cn

#### 创建证书存储目录

```bash
mkdir -p /home/xedu/ssl/cnbma.cn
```

#### 申请证书

```bash
acme.sh --dns dns_tencent --issue -d *.cnbma.cn -d cnbma.cn
```

#### 安装证书

```bash
acme.sh --installcert -d *.cnbma.cn \
        --key-file /home/xedu/ssl/cnbma.cn/*.cnbma.cn.key \
        --fullchain-file /home/xedu/ssl/cnbma.cn/fullchain.cer \
        --reloadcmd "/usr/sbin/nginx -s reload"
```

## 自动续期

acme.sh 已经自带定时任务功能，无需额外设置。证书每 60 天会自动更新一次。