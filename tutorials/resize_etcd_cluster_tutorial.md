---
title: Resize etcd cluster
description: This tutorial explains how to resize an etcd cluster.
---

### Resize etcd cluster

In my-etcd/etcd-cluster.yaml the initial cluster size is 3. Modify the file and change size from 3 to 5.

Execute below command to check current `etcd-cluster.yaml` and pods status:

```execute
cat etcd-cluster.yaml
```

Sample output:

```
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 3
  version: 3.2.13
```

```execute
kubectl get pods -n my-etcd
```

Sample output:

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-7rkk5   3/3     Running   0          37m
example-h9wk7l2hp4               1/1     Running   0          35m
example-nnjcbwsdrn               1/1     Running   0          36m
example-trgkbsl8lf               1/1     Running   0          36m
```

**Note - Please wait till `Status` will be `Running` and `READY` should be 1/1 or as per defined instances , and then proceed further.**

Execute below command to resize cluster:

```execute
cat <<'EOF' > etcd-cluster.yaml
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 5
  version: 3.2.13
EOF
```

Sample output:

```
etcdcluster.etcd.database.coreos.com/example configured
```

```execute
kubectl apply -f etcd-cluster.yaml -n my-etcd
```

Execute below command to check pods status:

```execute
kubectl get pods -n my-etcd
```

Sample Output:

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-r8mj2   3/3     Running   0          13m
example-57zlcvcrpp               1/1     Running   0          12m
example-g9rktt662f               1/1     Running   0          12m
example-j5nr8kmcgx               1/1     Running   0          11m
example-kbp7vtcsrd               1/1     Running   0          74s
example-sklwmxg5lv               1/1     Running   0          34s
```
**Note - Please wait till `Status` will be `Running` and `READY` should be 1/1 or as per defined instances , and then proceed further.**
