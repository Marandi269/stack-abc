### create stack user

```
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

sudo su - stack

```

```
export https_proxy=http://192.168.56.1:7890 http_proxy=http://192.168.56.1:7890 all_proxy=socks5://192.168.56.1:7890 no_proxy='192.168.56.17'
```



```
#!/bin/bash
export https_proxy=http://192.168.56.1:7890 http_proxy=http://192.168.56.1:7890 all_proxy=socks5://192.168.56.1:7890 no_proxy='192.168.56.17'

sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

echo "nameserver 8.8.8.8" |sudo tee /etc/resolv.conf
cp /etc/apt/sources.list /tmp
sudo cat > /etc/apt/sources.list << EOF
deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
EOF

sudo apt update && sudo apt upgrade -y && sudo apt install -y ntpdate git meson build-essential libcap-dev xsltproc
sudo timedatectl set-timezone Asia/Shanghai
sudo ntpdate cn.pool.ntp.org
sudo sed -i 's@net.ipv6.conf.all.disable_ipv6 = 1@net.ipv6.conf.all.disable_ipv6 = 0@' /etc/sysctl.conf 
sudo sysctl -p
git clone https://github.com/iputils/iputils
cd iputils && ./configure && meson build
cd builddir && sudo meson install
cd ~


```

```bash
#!/bin/bash
cd ~
export https_proxy=http://192.168.56.1:7890 http_proxy=http://192.168.56.1:7890 all_proxy=socks5://192.168.56.1:7890 no_proxy='192.168.56.17'

git clone https://git.openstack.org/openstack-dev/devstack
cd devstack

sed -i 's@OVS_RUNDIR=$OVS_PREFIX/var/run/openvswitch@OVS_RUNDIR=$OVS_PREFIX/var/run/ovn@' ./lib/neutron_plugins/ovn_agent

sed -i 's@arping@/usr/local/bin/arping@' ./lib/neutron-legacy
cat > local.conf << EOF
[[local|localrc]]
ADMIN_PASSWORD=admin
ADMIN_PASSWORD=\$ADMIN_PASSWORD 
DATABASE_PASSWORD=\$ADMIN_PASSWORD
RABBIT_PASSWORD=\$ADMIN_PASSWORD
SERVICE_PASSWORD=\$ADMIN_PASSWORD
HOST_IP=192.168.56.17
PUBLIC_INTERFACE=eth1
FLOATING_RANGE=192.168.56.224/27
EOF
./stack.sh




```

```
[[local|localrc]]
ADMIN_PASSWORD=admin

ADMIN_PASSWORD=$ADMIN_PASSWORD 
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
HOST_IP=192.168.56.16

PUBLIC_INTERFACE=eth1
FLOATING RANGE=192.168.56.224/27


```



### run debian 11 in openstack

https://computingforgeeks.com/run-debian-11-bullseye-on-openstack-kvm/