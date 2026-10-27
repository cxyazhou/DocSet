# 虚拟机安装Ubuntu
## 终端打不开
1. ctrl+alt+F3，直接执行 su，输入密码后能以root身份登录
2. nano /etc/default/locale
```
把LANG这行的值修改为：en_US.UTF-8
使用ctrl+s保存， ctrl+x退出
```
3. 重启即可
## 配置静态ip
### 网络改为桥接模式启动，然后直接在界面上修改后重新开关网络连接
### 界面修改失败后使用配置文件方式修改
1. sudo apt install net-tools
2. sudo chmod 600 /etc/netplan/01-network-manager-all.yaml
3. 修改文件内容/etc/netplan/01-network-manager-all.yaml
```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s3:
      dhcp4: false
      dhcp6: false
      addresses: [192.168.11.56/24]
      gateway4: 192.168.11.1
      nameservers:
        addresses: [172.16.10.9]
```
4. sudo netplan apply
5. 关机，网络改为桥接模式，再次开机即可
## ssh
```
#apt install openssh-server
#vi /etc/ssh/sshd_config
PermitRootLogin yes
StrictModes yes
#service ssh restart
```
## tftp
```
#apt install tftp-hpa tftpd-hpa
#mkdir /tftpboot
#chmod 777 -R /tftpboot
#vi /etc/default/tftpd-hpa
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/tftpboot"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="-l -c -s"
#service tftpd-hpa restart
```
## nfs
```
#apt install nfs-kernel-server
#vi /etc/exports
/tftpboot *(rw,sync,no_subtree_check,no_root_squash)
#mkdir -p /tftpboot
#chmod 777 /tftpboot
解决某些客户端挂载时提示Protocol not supported的问题，注意可能会导致某些客户端挂不上（Connection refused）：
vi /etc/default/nfs-kernel-server，末尾加上RPCNFSDOPTS="–nfs-version 2,3,4 --debug --syslog"
#service nfs-kernel-server restart
```
## 安装32位兼容包
```
注意：下面这2行不执行的话，代码编译会有问题
rm /bin/sh
ln -s /bin/bash /bin/sh
apt install aptitude
aptitude install lib32z1 lib32stdc++6 binutils
```
## 安装基础包
```
apt install mtd-utils zlibc make
```
## 自动登录root
```
#vi /etc/pam.d/gdm-autologin
注释行 “auth requied pam_succeed_if.so user != root quiet success”
#vi /etc/pam.d/gdm-password
注释行 “auth requied pam_succeed_if.so user != root quiet success”
#vi /root/.profile
找到“mesg n”，将其更改为tty -s && mesg n
修改/etc/gdm3/custom.conf 文件
修改[daemon]选项
AutomaticLoginEnable=true
AutomaticLogin=root
TimedLoginEnable=true
修改[security]
AllowRoot=true
```
## 修改源：
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
sudo vi /etc/apt/sources.list
ubuntu的网址，比如http://archive.ubuntu.com/ubuntu http://security.ubuntu.com/ubuntu
替换为
http://mirrors.aliyun.com/ubuntu
```
## 关闭自动更新
```
#vi /etc/apt/apt.conf.d/20auto-upgrades
1改成0
```
## 安装软件
```
apt install lzop u-boot-tools
```