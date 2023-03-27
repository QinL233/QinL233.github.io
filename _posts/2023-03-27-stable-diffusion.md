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
python3 launch.py --skip-torch-cuda-test --upcast-sampling --use-cpu interrogate
#bash webui.sh --skip-torch-cuda-test --precision full --no-half --use-cpu Stable-diffusion GFPGAN ESRGAN VAE --all --shar
```

问题：debian apt update时NOT_KEY需去手动下载证书：https://debian.pkgs.org/11/debian-main-amd64/ca-certificates_20210119_all.deb.html
```

#安装证书
dpkg -i 证书.deb

#安装证书时依赖报错
sudo apt-get -f install
```
