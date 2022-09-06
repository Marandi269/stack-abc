## 创建宿主机

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  # 映射ssh端口到宿主机 方便从外部连接
  config.vm.network "forwarded_port", guest: 22, host: 22222, host_ip: "0.0.0.0"
  config.vm.network "private_network", ip: "192.168.56.20"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "16384"
    vb.cpus = "16"
     
    # 开启嵌套虚拟化 
    vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"] 
  end
end

```

## 创建用户并进入

```
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

sudo su - stack
```



## 更新系统

```
sudo apt update &&sudo apt upgrade -y && sudo apt install -y ntpdate git
sudo timedatectl set-timezone Asia/Shanghai
sudo ntpdate cn.pool.ntp.org

```

## 获取代码

```bash
git clone https://git.openstack.org/openstack-dev/devstack
# 当前使用的版本

# commit b5c2e7b3fac7e603979fbdf52375154bf932c0f6 (HEAD -> master, origin/master, origin/HEAD)
# Merge: 3de92db6 b9b6d6b8
# Author: Zuul <zuul@review.opendev.org>
# Date:   Tue Aug 30 22:53:05 2022 +0000
#     Merge "Respect constraints on tempest venv consistently"

```

## 添加配置

```
cd devstack
vim local.conf
```

内容如下

>[[local|localrc]]
>
>export ADMIN_PASSWORD=admin
>
>export MY_IP=192.168.56.20

>ADMIN_PASSWORD=$ADMIN_PASSWORD
>DATABASE_PASSWORD=$ADMIN_PASSWORD
>RABBIT_PASSWORD=$ADMIN_PASSWORD
>SERVICE_PASSWORD=$ADMIN_PASSWORD
>
>HOST_IP=$MY_IP

```
bugfix:

sed -i 's@OVS_RUNDIR=$OVS_PREFIX/var/run/openvswitch@OVS_RUNDIR=$OVS_PREFIX/var/run/ovn@' ./lib/neutron_plugins/ovn_agent

sudo sed -i 's@net.ipv6.conf.all.disable_ipv6 = 1@net.ipv6.conf.all.disable_ipv6 = 0@' /etc/sysctl.conf 
sudo sysctl -p

```



> ERROR /opt/stack/devstack/lib/neutron_plugins/ovn_agent:174 Socket
>
> https://stackoverflow.com/questions/68001501/error-opt-stack-devstack-lib-neutron-plugins-ovn-agent174-socket

