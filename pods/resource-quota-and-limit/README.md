**ResourceQuotas and Limits are mechanisms to control how much cluster resources (like CPU, memory) your workloads can consume.**

**1. Resource Quota**
A ResourceQuota is set at the namespace level. It controls the total amount of cluster resources (CPU, memory, storage, etc.) that can be consumed in that namespace.

Think of it like a budget:
- You define how many CPUs/memory the entire namespace can use.
- You can limit the number of Pods, Services, PersistentVolumeClaims, etc.

This means:
- Max 4 CPU requested, 8 CPU limited.
- Max 8Gi memory requested, 16Gi limited.
- Up to 10 Pods, 5 PVCs.

**2. Resource Limits (LimitRange)**
A LimitRange sets default resource requests/limits per Pod/container in a namespace. It can:
- Apply default values if none are specified.
- Enforce minimum and maximum resource usage per container.

This means:
- Each container can't exceed 2 CPU/4Gi memory.
- At least 100m CPU/256Mi memory must be requested.
- If you don't specify requests/limits in your Pod spec, it will use the defaults.

<pre><code>


apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 200m
            memory: 256Mi   
</code></pre>

<pre><code>kubectl describe pod pod-name</code></pre> 
