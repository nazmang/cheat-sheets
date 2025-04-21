
This cheat sheet contains examples of commonly used Kubernetes workloads with short descriptions and helpful notes.

---

## Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-kv
  namespace: stage
  labels:
    name: redis-kv
    app: redis
spec:
  containers:
  - name: redis
    image: redis
  - name: redis-exporter
    image: bitnami/redis-exporter
```

### Description

A **Pod** is the smallest deployable unit in Kubernetes. It represents a single instance of a running process in the cluster.

### Notes

- Pods are ephemeral; they can be replaced or rescheduled by controllers.
- For high availability, use a **Deployment** or **StatefulSet** instead of standalone Pods.

---

## Namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: stage
  labels:
    name: stage
---
apiVersion: v1
kind: Namespace
metadata:
  name: dev
  labels: 
    name: dev    
```

### Description

**Namespaces** provide a way to logically separate workloads within a cluster.

### Notes

- Use namespaces to separate development, testing, and production environments.
- Some Kubernetes resources (like Nodes and PersistentVolumes) are **not** namespaced.

---

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-dp
  namespace: dev
spec:
  replicas: 2
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 1
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
        - name: httpd
          image: httpd
```

### Description

A **Deployment** provides declarative updates for Pods and ReplicaSets.

### Notes

- Supports **three** update strategies:
    - `RollingUpdate` (default) - updates Pods gradually.
    - `Recreate` - stops existing Pods before creating new ones.
    - `Blue-Green`/`Canary` - managed externally with separate Deployments.
- Use `maxUnavailable` and `maxSurge` to control rollout speed.

---

## DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-ds
  namespace: default
spec:
  selector:
    matchLabels:
      app: nginx-ds
  template:
    metadata:
      labels:
        app: nginx-ds
    spec:
      nodeSelector:
        usecase: loadbalancer
      containers:
      - name: nginx
        image: nginx
```

### Description

A **DaemonSet** ensures a copy of a Pod runs on every (or selected) node.

### Notes

- Used for system services like logging, monitoring, or networking.
- Can be scheduled on specific nodes using `nodeSelector`, `nodeAffinity`, or `tolerations`.

---

## Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: test-job
spec:
  template:
    metadata:
      labels:
        app: ping-google
    spec:
      restartPolicy: OnFailure
      containers:
      - name: main
        image: alpine
        command: ["ping"]
        args: ["-c", "4", "8.8.8.8"]
```

### Description

A **Job** runs a task to completion and tracks successful executions.

### Notes

- Set `restartPolicy: OnFailure` to retry on failure.
- Use `parallelism` and `completions` to control job execution.

---

## CronJob

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: cron-test
  namespace: default
spec:
  schedule: "*/10 * * * *"
  jobTemplate:
    spec:
      template:
        metadata:
          labels:
            app: cron-test
        spec:
          restartPolicy: OnFailure
          containers:
          - name: alpine-cronjob
            image: alpine
            command: ["curl"]
            args: ["https://my.server.com/kubernetes/report"]        
```

### Description

A **CronJob** runs Jobs on a scheduled basis.

### Notes

- Uses cron syntax for scheduling.
- Set `successfulJobsHistoryLimit` and `failedJobsHistoryLimit` to manage history.

---

## Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: http-svc-int
  namespace: default
spec:
  type: ClusterIP
  selector:
    app.kubernetes.io/name: httpd-dp
  ports:
  - port: 80
    targetPort: 80
```

### Description

A **Service** exposes a set of Pods as a network service.

### Notes

- Service types:
    - `ClusterIP` - Internal access only.
    - `NodePort` - Exposes on each node’s IP and a static port.
    - `LoadBalancer` - Uses cloud provider’s LB.
    - `ExternalName` - Maps DNS to external services.
    - `Headless` (`ClusterIP: None`) - Allows direct Pod access.

---

## Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: http-svc-int
            port:
              number: 80
```

### Description

An **Ingress** manages external access to services inside a cluster.

### Notes

- Requires an **Ingress Controller** (e.g., Nginx, Traefik, HAProxy).
- Supports TLS termination and path-based routing.

---

## PersistentVolume & PersistentVolumeClaim

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-example
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

### Description

**PersistentVolumes (PV)** and **PersistentVolumeClaims (PVC)** provide persistent storage for Pods.

### Notes

- Storage types: NFS, Ceph, AWS EBS, GCE PD, etc.
- `ReadWriteOnce`, `ReadWriteMany`, and `ReadOnlyMany` define access modes.

---

