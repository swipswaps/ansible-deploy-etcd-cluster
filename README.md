### Example playbook:

ansible-playbook -i inventory.ini deploy.yml -vv


# ansible-deploy-etcd-cluster

| 集群大小   |    最大容错  |
| :-------- | :--:    |
| 1	      |    0      |
| 3	      |    1      |
| 5	      |    2      |
| 7	      |    3      |
| 9	      |    4      |

etcd 集群的搭建有三种方式：

* static 方式
* etcd discovery
* DNS discovery

|name	 | ip |
| :-------- | :--:    |
|etcd1 | 172.16.3.111 |
|etcd2 | 172.16.3.112 |
|etcd3 | 172.16.3.113 |

static 方式


172.16.3.111：

$ etcd --name etcd1 --data-dir /vpants/etcd/data --initial-advertise-peer-urls http://172.16.3.111:2380 \
  --listen-peer-urls http://172.16.3.111:2380 \
  --listen-client-urls http://172.16.3.111:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://172.16.3.111:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcd1=http://172.16.3.111:2380,etcd2=http://172.16.3.112:2380,etcd3=http://172.16.3.113:2380 \
  --initial-cluster-state new
  
172.16.3.112:

$ etcd --name etcd2 --data-dir /vpants/etcd/data --initial-advertise-peer-urls http://172.16.3.112:2380 \
  --listen-peer-urls http://172.16.3.112:2380 \
  --listen-client-urls http://172.16.3.112:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://172.16.3.112:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcd1=http://172.16.3.111:2380,etcd2=http://172.16.3.112:2380,etcd3=http://172.16.3.113:2380 \
  --initial-cluster-state new
  
 172.16.3.113:
 
 $ etcd --name etcd3 --data-dir /vpants/etcd/data --initial-advertise-peer-urls http://172.16.3.113:2380 \
  --listen-peer-urls http://172.16.3.113:2380 \
  --listen-client-urls http://172.16.3.113:2379,http://127.0.0.1:2379 \
  --advertise-client-urls http://172.16.3.113:2379 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-cluster etcd1=http://172.16.3.111:2380,etcd2=http://172.16.3.112:2380,etcd3=http://172.16.3.113:2380 \
  --initial-cluster-state new

# etcdctl 

# show cluster node
etcdctl member list

# update cluster node
etcdctl member update id  http://ip:2380

# delete cluster node
etcdctl member reomve id

# add cluster node
etcdctl member add name http://ip:2380
>  initial-cluster-state: existing
