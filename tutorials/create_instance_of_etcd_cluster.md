---
title: etcd Operator Tutorial to create an instance of etcd cluster
description: This tutorial explains how to create an instance of etcd cluster
---

### Create Instance of etcd Cluster

etcd-Operator Instance can be created using the Custom Resource Definition YAML files.

**step 1:** Create a custom resource YAML file

```execute
cat <<'EOF' >etcd-cluster.yaml
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 3
  version: 3.2.13
EOF
```

**Step 2:** create etcd-cluster in the same namespace of operator.

```execute
kubectl create -f etcd-cluster.yaml -n my-etcd
```

Sample output:

```
etcdcluster.etcd.database.coreos.com/example created
```

**Step 3:** Check etcd-cluster pods.

```execute
kubectl get pods -n my-etcd
```

You will see output similar like below:

```
NAME                             READY   STATUS     RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running    0          11m
example-7m2dq8mk5d               1/1     Running    0          45s
example-xdsgpp9c6s               0/1     Init:0/1   0          24s
```

It may take up to a few minutes until all the resources are created and the cluster is ready for use.

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running   0          30m
example-7m2dq8mk5d               1/1     Running   0          20m
example-vfbs98thmq               1/1     Running   0          19m
example-xdsgpp9c6s               1/1     Running   0          19m
```

**Note - Please wait till `Status` will be `Running` and `READY` should be 1/1 , and then proceed further.**

### Accessing etcd-cluster from within a pod/container

**Step 1:** Open the cluster-service

```execute
kubectl get svc -n my-etcd
```

You will see output similar below:

```
NAME                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
etcd-restore-operator   ClusterIP   10.96.179.156    <none>        19999/TCP           13m
example                 ClusterIP   None             <none>        2379/TCP,2380/TCP   2m38s
example-client          ClusterIP   10.107.108.163   <none>        2379/TCP            2m38s
```

To access etcd cluster externally, lets first update service to use NodePort:

* Execute below command to use NodePort:
```execute
kubectl get service example-client --output yaml -n my-etcd > /tmp/my-etcd.yaml
sed -i "s/type: .*/type: NodePort/g" /tmp/my-etcd.yaml
kubectl patch svc example-client -p "$(cat /tmp/my-etcd.yaml)" --namespace my-etcd
```

* Execute below command to update NodePort to 32379:
```execute
kubectl get service example-client --output yaml -n my-etcd > /tmp/my-etcd.yaml
sed -i "s/nodePort: .*/nodePort: 32379/g" /tmp/my-etcd.yaml
kubectl patch svc example-client -p "$(cat /tmp/my-etcd.yaml)" --namespace my-etcd
```


**Step 2:** Connect to etcd-cluster from etcd-client container

```execute
kubectl run --rm -i --tty etcd-client --image quay.io/coreos/etcd --restart=Never  --env ClusterIP=##DNS.ip##  -- /bin/sh
```

You will see output similar below:

```
If you don't see a command prompt, try pressing enter.
/ #
```

**Step 3:** Put sample key-value pair into etcd-cluster database

***NOTE: We set environment variable ETCDCTL_API=3 to use v3 API(ETCDCTL_API=2 to use v2 API). If not set will give a warning.***

Execute below command to put key-value pair.

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##DNS.ip##:32379 put cluster admin
```

Sample output:

```
OK
```

**Step 4:** Retrieving the value by key

Execute below command to retrive value of key

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##DNS.ip##:32379 get cluster 
```

Sample Output:

```
cluster
admin
```

Execute below command to exit out of terminal:

```execute
exit
```
