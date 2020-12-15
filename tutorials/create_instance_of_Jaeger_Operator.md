---
title: Jaeger Operator Tutorial to create an instance of Jaeger Operator
description: This tutorial explains how to create an instance of Jaeger Operator
---

### Create Instance of Jaeger Operator

Jaeger Operator Instance can be created using the Custom Resource Definition YAML files.

**step 1:** Create a custom resource YAML file

```execute
cat <<'EOF' >jaegerInstance.yaml
apiVersion: jaegertracing.io/v1
kind: Jaeger
metadata:
  name: jaeger-all-in-one-inmemory
EOF
```

**Step 2:** create Jaeger Operator in the namespace observability.

```execute
kubectl create -f jaegerInstance.yaml -n observability
```

**Step 3:** Check the Jaeger Instance.
```execute
 kubectl get jaegers
```
output:
```
NAME        CREATED AT
simplest    28s
```

**Step 4:** Check the pods.

```execute
kubectl get pods -n observability
```

output:

```
NAME                        READY     STATUS    RESTARTS   AGE
simplest-6499bb6cdd-kqx75   1/1       Running   0          2m
```

