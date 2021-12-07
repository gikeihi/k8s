# 安装linux
在虚拟机上安装linux 7.9
## 打开root的远程登录
```bash
$ vi /etc/ssh/sshd_config
```
修改
```
PermitRootLogin no
```
为
```
PermitRootLogin yes
```
重启服务
```bash
$ systemctl restart sshd.service
```
## 如果在安装过程中没有创建用户，可以用以下命令创建
```bash
$ adduser username
$ passwd username
$ usermod -aG wheel username
$ su - username
$ sudo ls -la /root
```
## 如果在安装过程中忘记修改默认主机名的话，可以用以下命令修改
```bash
$ hostnamectl status                              # 查看当前的hostnmae
$ hostnamectl set-hostname newhostname            # 使用这个命令会立即生效且重启也生效
$ echo "127.0.0.1   $(hostname)" >> /etc/hosts    # 设置 hostname 解析
```
## 如果在安装过程中没有设置静态IP，可以按照以下方式进行设置
```bash
$ ip route show                                   # 检查网络查看机器的默认显卡通常是eth0
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0    # 根据默认显卡名称编辑对应的文件
```
如下修改
```
BOOTPROTO=static          #设置网卡获得ip地址的方式，可能的选项为static，dhcp或bootp，分别对应静态指定的 ip地址，通过dhcp协议获得的ip地址，通过bootp协议获得的ip地址
IPADDR=192.168.11.201      #如果设置网卡获得 ip地址的方式为静态指定，此字段就指定了网卡对应的ip地址
GATEWAY=192.168.11.1      #(设置本机连接的网关的IP地址。)
NETMASK=255.255.255.0     #网卡对应的网络掩码
DNS1=192.168.11.1
```
重启服务
```bash
$ sudo systemctl restart network 
```
# k8s
## 各个工作节点都要安装
```bash
curl -sSL https://raw.githubusercontent.com/gikeihi/k8s/main/install_kubelet.sh | sh -s 1.22.3
```
## Master节点初始化
```bash
# 替换 x.x.x.x 为 master 节点的内网IP
export MASTER_IP=192.168.11.201
echo "${MASTER_IP}    cluster-endpoint" >> /etc/hosts
curl -sSL https://raw.githubusercontent.com/gikeihi/k8s/main/init_master.sh | sh -s 1.22.3
```