# Jaeger Operator
### Overview
Jaeger, inspired by Dapper and OpenZipkin, is a distributed tracing system released as open source by Uber Technologies. It is used for monitoring and troubleshooting microservices-based distributed systems.

### Core capabilities:
Jaeger is used for monitoring and troubleshooting microservices-based distributed systems, including:

- Distributed context propagation
- Distributed transaction monitoring
- Root cause analysis
- Service dependency analysis
- Performance / latency optimization
- OpenTracing compatible data model
- Multiple storage backends: Badger, Cassandra, Elasticsearch, Memory.

**Operator features:**

- Multiple modes - Supports allInOne, production and streaming modes of deployment.

- Configuration - The Operator manages configuration information when installing Jaeger instances.

- Storage - Configure storage used by Jaeger. By default, memory is used. Other options include badger, cassandra or elasticsearch. On OpenShift, the operator can delegate creation of an Elasticsearch cluster to the Elasticsearch Operator if deployed.

- Agent - can be deployed as sidecar (default) and/or daemonset.

- UI - Optionally setup ingress (Kubernetes) or secure route (OpenShift) to provide access to the Jaeger UI.


