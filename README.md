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