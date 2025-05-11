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


**NO Metrics avaiable in kind-cluster**

![Metrics-Not-Available](https://github.com/herrry107/Kubernetes/blob/main/images/metrics-not-available.png)

We have to create metrics 
<pre><code>
#run in minikube only
minikube addons enable metrics-server
</code></pre>

If you are using a Kind cluster install Metrics Server
<pre><code>
# run in kind-cluster only
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml  
</code></pre>

Edit the Metrics Server Deployment
<pre><code>
kubectl -n kube-system edit deployment metrics-server
</code></pre>

![Metrics-add-ssl-bypass](https://github.com/herrry107/Kubernetes/blob/main/images/metrics-step1.png)

Add the security bypass to deployment under container.args
<pre><code>
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
</code></pre>

![Metrics-add-ssl-bypass](https://github.com/herrry107/Kubernetes/blob/main/images/metrics-step2.png)
  
Restart the deployment
<pre><code>
kubectl -n kube-system rollout restart deployment metrics-server
</code></pre>

Verify if the metrics server is running
<pre><code>
kubectl get pods -n kube-system
kubectl top nodes
</code></pre>

![Metrics-add-ssl-bypass](https://github.com/herrry107/Kubernetes/blob/main/images/metrics-step3.png)

hpa.yml
<pre><code>
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: apache-hpa
  namespace: apache
spec:
  scaleTargetRef:
    kind: Deployment
    name: apache-deployment
    apiVersion: apps/v1
     
  minReplicas: 1
  maxReplicas: 5

  metrics:
    - type: Resource
      resource:
        name: cpu
        target: 
          type: Utilization
          averageUtilization: 5    
</code></pre>
<pre><code>kubectl apply -f hpa.yml</code></pre>
<pre><code>kubectl get hpa -n apache</code></pre>

![Metrics](https://github.com/herrry107/Kubernetes/blob/main/images/metrics-step4.png)
