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
```

## 软件包安装
```
yum -y install ansible vim git
```

## 拉取代码
```
git clone git@gitee.com:chriscentos/system-init.git
```

## 复制模版信息
```
\cp group_vars/all-template group_vars/all
\cp hosts-example hosts
```

## 配置主机列表
```
[nodes01]
192.168.1.10 

[all:vars]
ansible_ssh_port=22
ansible_ssh_user=root
ansible_ssh_pass="bkce123"
```

## 请您优先配置脚本初始化变量
```
vim group_vars/all
# system init size percentage
ntp_server_host: 'ntp1.aliyun.com' #配置NTP地址
dns_server_host: '114.114.114.114' #配置DNS地址

# Safety reinforcement related configuration
auth_keys_file: '/root/.ssh/authorized_keys' #设置key文件地址
password_auth: 'yes' #是否开启密码登录，yes表示开启 no表示不开启
root_public_key: ''  #设置免密公钥
root_passwd: 'bkce123'
```

## 服务器优化脚本执行方式
```
ansible-playbook playbooks/system_init.yml -e "nodes=nodes01"
```

