---
title: How to to Add Users and Enable Authentication
description: This tutorial explains how to to Add Users and Enable Authentication
---
### Add Users and Enable Authentication
Connect to etcd-cluster locally using kubectl

```execute
kubectl run --rm -i --tty etcd-client --image quay.io/coreos/etcd --restart=Never  --env ClusterIP=##DNS.ip##  -- /bin/sh
```

**Step 1:** Create user for etcd-cluster. 

***NOTE: We set environment variable ETCDCTL_API=3 to use v3 API(ETCDCTL_API=2 to use v2 API). If not set will give a warning.***

Execute below command to add user for etcd-cluster . It will prompt for password which you need to enter and confirm.

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##DNS.ip##:32379 user add etcd-user
```

Sample output:

```
Password of openlabs:
Type password of etcd-user again for confirmation:
User etcd-user created
```

**Step 2:** Checking the user creating is successful or not

Execute below command to check created usuer:

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##DNS.ip##:32379 user list
```

Sample output:

```
etcd-user
```

Execute below command to exit out of terminal:

```execute
exit
```
