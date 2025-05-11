# Auto Scaling

Kubernetes can automatically adjust the number of pods based on CPU/memory usage or custom metrics.

![Alt-text](https://github.com/herrry107/Kubernetes/blob/main/images/scalling.png)

# Horizontal Pod Autoscaler (HPA)

Scales OUT or IN by changing the number of pod replicas.

What it does:
- Monitors CPU, memory, or custom metrics.
- Adjusts the number of pods in a Deployment/ReplicaSet.

Use when:
- Your app can handle load by running more instances.
- You're handling variable traffic patterns.

# Vertical Pod Autoscaler (VPA)

Scales UP or DOWN by adjusting the resources (CPU/memory) of individual pods.

What it does:
- Recommends or sets resource requests/limits.
- Optionally evicts and restarts pods with new resources.

Use when:
- Your app is not horizontally scalable (e.g., monoliths).
- You want to optimize resource requests over time.


