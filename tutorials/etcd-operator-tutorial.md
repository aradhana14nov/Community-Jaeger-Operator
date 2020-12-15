---
title: etcd Operator Tutorial
description: Check Operator Deployment Status
---

### Check the etcd Operator creation status 

Execute the following command to check if the etcd Operator is running:
```execute
kubectl get csv -n my-etcd | egrep -i "name|etcd"
```

Sample output:

```
NAME                             DISPLAY                       VERSION       REPLACES                   PHASE
elastic-cloud-eck.v1.0.0-beta1   Elastic Cloud on Kubernetes   1.0.0-beta1   elastic-cloud-eck.v0.9.0   Succeeded
etcdoperator.v0.9.4              etcd                          0.9.4         etcdoperator.v0.9.2        Succeeded

```

**Note - Please wait till `PHASE` will be `Succeeded` and then proceed further.**

