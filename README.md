# Dockin Installer - Dockin Platform Installer

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

English | [中文（推荐）](README.zh-CN.md)

The Dockin platform installer supports the rapid deployment of highly available kubernetes clusters and ETCD clusters. Possess production-level parameter tuning capabilities.

**For more Dockin components, please visit [https://github.com/WeBankFinTech/Dockin](https://github.com/WeBankFinTech/Dockin)**

![Architecture](docs/images/dockin.png)

## Features

* **0.1.0**
  * Support ETCD high availability offline deployment
  * Support Kubernetes high availability offline deployment
  * Support Docker offline deployment
  * Turn off kernel memory accounting
  * Full link supports HTTPS
  * 10 years certificate signature

## Installation

### Minimum Requirements

* **OS**
    * centos ≥  7
    * kernel ≥ 3.10

## QuickStart

### download release package

- download release package

### Install ETCD

- Unzip to directory：dockin-etcd
- Default deployment path：/data/app/dockin-etcd
- Certificate generation path：/data/app/dockin-etcd/conf
- Configuration file：conf/install.properties
- Default port：5379
- Install Command

```
Modify the configuration file: vi conf/install.properties
Fill in the parameters according to the format: server_list=(ip1 ip2 ip3)
```

```
sudo ./install.sh 
```

### Install Docker

- Unzip to directory：dockin-docker
- Configuration file：none
- Install Command

```
cd dockin-docker
sudo ./install.sh
```

### Install WORKER

- Unzip to directory：dockin-worker
- Configuration file：conf/install.properties

```
ip=[@HOSTIP]

# Token added to the cluster, generated by the master script
token=[#join-token]

# Master ApiServer IP/VIP
master=[#master-vip]
```

- Install Command

```
cd dockin-worker
# If the non-master node master_node parameter needs to be changed to false
sudo ./install.sh install v1.16.6 master_node=true

```


### Install K8S Master

- Unzip to directory：dockin-master
- Configuration file：conf/install.properties

```
# master HA VIP
master_vip=[#MASTER_VIP]

# masterIP and VIP
master_ip_list=[#MASTER_IP_LIST]

# local IP
local_ip=[@HOSTIP]

# etcd list, eg: https://ip1:port1,https://ip2:port2,https://ip3:port3; 
# Please note that the port of dockin-etcd is 5379
etcd_list=[#ETCD_LIST]
```

- ETCD Certificate path：/etc/kubernetes/pki/etcd/

```
# Need to include the following files, obtained from the ETCD node /data/app/dockin-etcd/conf path
ca.pem client.pem client-key.pem
```

- Install Command

```
cd dockin-master
# If it is not the first node, please set first_node to false
sudo ./install.sh install v1.16.6 first_node=true
```

### Use an external load balancer

Use the LB provided by cloud vendors, self-built haproxy, and self-built nginx to access apiserver as a highly available load balancer
