### **1. Node Affinity** Позволяет управлять размещением подов на узлах кластера с помощью `nodeSelectorTerms` и `operator` (In, NotIn, Exists и др.).

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: disktype
                operator: In
                values:
                  - ssd
```

### **2. Requests / Limits** Определяют запрашиваемые (`requests`) и предельные (`limits`) ресурсы для контейнера.

```yaml
resources:
  requests:
    cpu: "500m"  # 0.5 vCPU
    memory: "256Mi"
  limits:
    cpu: "1"     # 1 vCPU
    memory: "512Mi"
```

### **3. LimitRange / ResourceQuota**

- `LimitRange` ограничивает ресурсы для подов/контейнеров в namespace.
    

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

- `ResourceQuota` ограничивает суммарное потребление ресурсов в namespace.
    

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
spec:
  hard:
    requests.cpu: "2"
    requests.memory: "1Gi"
    limits.cpu: "4"
    limits.memory: "2Gi"
```

### **4. Pod Disruption Budget (PDB)** Определяет минимальное количество работающих подов во время обновлений или автоматических эвакуаций.

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: pdb-example
spec:
  minAvailable: 2  # Либо `maxUnavailable: 1`
  selector:
    matchLabels:
      app: my-app
```

### **5. NetworkPolicy** Контролирует сетевой трафик между подами.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-specific
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 80
```

### **6. Horizontal / Vertical Auto Scaling**

- `HPA` автоматически масштабирует количество подов.
    

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-example
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 80
```

- `VPA` изменяет `requests/limits` у подов в зависимости от нагрузки.
    

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: vpa-example
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind: Deployment
    name: my-deployment
  updatePolicy:
    updateMode: "Auto"
```