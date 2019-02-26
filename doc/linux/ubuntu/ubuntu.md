# Ubuntu使用

## ubuntu设置网络

https://help.ubuntu.com/lts/serverguide/network-configuration.html.en

## netplan使用

https://netplan.io/examples

## 修改DNS

Ubuntu 18 LTS netplan 网络配置

直接配置netplan吧

```Shell
vim /etc/netplan/50-cloud-init.yaml 

# 配置如下：

network:
    ethernets:
        enp4s0:
            addresses: [192.168.0.20/24]  //IP址
            gateway4: 192.168.0.1  // 网关
            nameservers:
             addresses: [114.114.114.114, 192.168.0.1] //DNS
            dhcp4: no
            optional: no
    version: 2
```

启用生效

```Shell
sudo netplan apply
```

http://www.cnblogs.com/abeen/p/9355493.html

## apt使用

搜索软件

sudo apt-cache search postgresql-

卸载软件

sudo apt-get remove postgresql-10

安装

sudo apt-get install postgresql-10

