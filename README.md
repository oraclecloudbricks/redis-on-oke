# Redis on Kubernetes

- [Redis on Kubernetes](#redis-on-kubernetes)
  - [Implementation](#implementation)
  - [Check replication status](#check-replication-status)

Implementation is based on the following documentation: 

- [Redis Operator](https://operatorhub.io/operator/redis-operator)
- [Redis Operator Documentation](https://ot-container-kit.github.io/redis-operator/guide/#supported-features)
- [Redis quickstart](https://docs.redis.com/latest/kubernetes/deployment/quick-start/)
- [Redis Implementation](https://www.containiq.com/post/deploy-redis-cluster-on-kubernetes)


---

## Implementation

1. Create namespace for redis

```shell
kubectl create namespace rddatabase
namespace/rddatabase created
```

2. Create config map for redis

```shell
kubectl apply -f 01_configmap.yaml 
configmap/redis-config created
```


**NOTE**

Config Map applied contains the Redis Password to be injected in cluster. This is included on line `15` and `16` of file `01_configmap.yaml` Update this up to what's required. 

3. Create redis database

```shell
kubectl apply -f 02_redis.yaml 
statefulset.apps/redis created
service/redisservice created
```

---

## Check replication status

1. ssh into `redis-0 pod

2. Run the following command: 

```shell
/data # redis-cli 
127.0.0.1:6379> auth W3lc0mn31.
127.0.0.1:6379> auth W3lc0m31.
OK
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:2
slave0:ip=10.244.4.21,port=6379,state=online,offset=308,lag=1
slave1:ip=10.244.4.143,port=6379,state=online,offset=308,lag=1
master_failover_state:no-failover
master_replid:f21452d8e3ed30a458ee2b8b9f561750a73977c5
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:308
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:308
```
