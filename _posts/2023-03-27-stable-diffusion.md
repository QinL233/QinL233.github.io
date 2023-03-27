---
title: 'stable-diffusion'
layout: post
tags:
  - ai
category: ai
---
stable-diffusion试用

<!--more-->

# 一、linux安装

## debian 11(Bullseye)环境
```
#1、安装必要依赖
apt update
apt install -y wget git gcc sudo libgl1 libglib2.0-dev python3-dev


#2、创建一个用户和空间（webui执行必须为非root）
useradd --home /app -M app -K UID_MIN=10000 -K GID_MIN=10000 -s /bin/bash
mkdir /app
chown app:app -R /app
adduser app sudo
echo 'app ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

#3、安装conda管理python
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-$(uname -m).sh
bash ./Miniconda3-latest-Linux-$(uname -m).sh -b

#4、安装stable-diffusion-webui
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git /app/stable-diffusion-webui

#5、修改launch.py
sed -i -E 's/\+?cu([0-9]{3})//g' /app/stable-diffusion-webui/launch.py
sed -i -E 's/torchvision==([^ ]+)/torchvision/g' /app/stable-diffusion-webui/launch.py

#6、配置临时环境变量并安装conda-python
export PATH=/app/miniconda3/bin/:$PATH
conda install python="3.10" -y
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

#7、进入stable-diffusion-webui并启动
cd /app/stable-diffusion-webui
#cpu执行
python3 launch.py --skip-torch-cuda-test --upcast-sampling --use-cpu interrogate
#bash webui.sh --skip-torch-cuda-test --precision full --no-half --use-cpu Stable-diffusion GFPGAN ESRGAN VAE --all --shar
```

## 问题：debian apt update时NOT_KEY
需去手动下载证书：https://debian.pkgs.org/11/debian-main-amd64/ca-certificates_20210119_all.deb.html
```

#安装证书
dpkg -i 证书.deb

#安装证书时依赖报错
sudo apt-get -f install
```

## 问题：stable-diffusion-webui依赖以及版本
```
#python > 3.10.9
#pytorch > 13.1.0
#直接在目录下安装所有依赖库：
pip3 install -r requirements.txt  
pip3 install -r requirements_versions.txt

#GFPGAN （腾讯开源的人脸识别模块）安装失败时
#官方安装地址：https://github.com/TencentARC/GFPGAN
git clone https://github.com/TencentARC/GFPGAN.git
cd GFPGAN
pip install basicsr  
pip install facexlib  
pip install -r requirements.txt  
python setup.py develop  
pip install realesrgan

#验证安装
➜  ~ python3  
Python 3.10.9 (main, Dec 15 2022, 17:11:09) [Clang 14.0.0 (clang-1400.0.29.202)] on darwin  
Type "help", "copyright", "credits" or "license" for more information.  
>>> import gfpgan  
>>>


```

## 问题：其他依赖timed out after 300030 milliseconds
编辑 launch.py 在github地址前增加http代理
```
https://ghproxy.com/

eg:git clone https://ghproxy.com/https://github.com/stilleshan/ServerStatus

```

# 二、模型使用
model地址：https://civitai.com/

将模型下载至```/stable-diffusion-webui/models/Stable-diffusion```并重启