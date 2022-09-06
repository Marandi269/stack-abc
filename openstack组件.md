

### devstack 自带组件

```bash
horizon  
keystone  
neutron  
novnc      
cinder       
glance
nova
placement
tempest
```

### skyline 安装

简单配置

```yaml
# data/skyline.yaml
default:
  database_url: sqlite:////tmp/skyline.db
  # database_url: mysql://skyline:SKYLINE_DBPASS@172.16.102.200:3306/skyline
  # prometheus_endpoint: http://172.16.102.200:9091

openstack:
  keystone_url: http://192.168.56.20/identity/v3/
  default_region: RegionOne
  interface_type: public
  system_user_name: skyline
  system_user_password: skyline


```

```
openstack user create --domain default --password skyline  skyline 
openstack role add --project service --user skyline admin
```



```bash


docker run --rm --name skyline_bootstrap -e KOLLA_BOOTSTRAP="" -v /tmp/skyline/skyline.yaml:/etc/skyline/skyline.yaml -v /tmp/skyline:/tmp --net=host 99cloud/skyline:latest

docker run -d --name skyline --restart=always -v /tmp/skyline/skyline.yaml:/etc/skyline/skyline.yaml -v /tmp/skyline:/tmp --net=host 99cloud/skyline:latest
```

