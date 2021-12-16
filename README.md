# system-init

## 系统环境
```
系统版本：CentOS Linux release 7.6.1810 (Core)
系统内核：3.10.0-957.el7.x86_64
```

## 配置YUM仓库
```
rm -f /etc/yum.repos.d/*.repo
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.cloud.tencent.com/repo/centos7_base.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.cloud.tencent.com/repo/epel-7.repo
yum repolist
```

## 软件包安装
```
yum -y install ansible vim git
```

## 拉取代码
```
cd /root/
git clone https://gitee.com/chriscentos/system-init.git
```

## 复制模版信息
```
cd /root/system-init/
\cp group_vars/all-template group_vars/all
\cp hosts-example hosts
```

## 配置主机列表
vim hosts
```
[nodes01]
192.168.1.10 

[all:vars]
ansible_ssh_port=22            ## 设置远程服务器端口
ansible_ssh_user=root          ## 设置远程服务器用户
ansible_ssh_pass="bkce123"     ## 设置远程服务器密码
```

## 生成密钥对
检查是否存在密钥对
```
# ls -l /root/.ssh/id_rsa*
-rw------- 1 root root 1679 Dec  3 22:51 /root/.ssh/id_rsa
-rw-r--r-- 1 root root  403 Dec  3 22:51 /root/.ssh/id_rsa.pub
```
若已生成密钥对，则可以跳过此操作
```
# ssh-keygen    ## 执行此命令，然后一直回车即可
```

## 初始化变量
vim group_vars/all
```
# system init size percentage
ntp_server_host: 'ntp1.aliyun.com' #配置NTP地址
dns_server_host: '114.114.114.114' #配置DNS地址

# Safety reinforcement related configuration
auth_keys_file: '/root/.ssh/authorized_keys' #设置key文件地址
password_auth: 'yes' #是否开启密码登录，yes表示开启 no表示不开启
root_public_key: ''  #设置免密公钥
root_passwd: 'bkce123'
```

## 服务器检查
检查远程服务器的SSH连接状态
```
ansible -m ping nodes01
192.168.1.10 | SUCCESS => {
```

## 服务器优化
```
ansible-playbook playbooks/system_init.yml -e "nodes=nodes01"
```

