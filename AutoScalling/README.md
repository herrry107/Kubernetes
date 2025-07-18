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

**first create deployment**
deployment.yml
<pre><code>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
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
            cpu: "50m"
          limits:
            cpu: "100m"
</code></pre>

**create service**
service.yml
<pre><code>
kind: Service
apiVersion: v1
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 31500
  type: NodePort
</code></pre>

# HPA

hpa.yml
<pre><code>
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    kind: Deployment
    name: nginx-deployment
    apiVersion: apps/v1
     
  minReplicas: 1
  maxReplicas: 5

  metrics:
    - type: Resource
      resource:
        name: cpu
        target: 
          type: Utilization
          averageUtilization: 5   #when exiding 5% than create new pod   
</code></pre>
<pre><code>kubectl apply -f hpa.yml</code></pre>
<pre><code>kubectl get hpa -n apache</code></pre>

![Metrics](https://github.com/herrry107/Kubernetes/blob/main/images/metrics-step4.png)

# VPA

firstly we have to configure some files for VPA 

clone kubernetes/autoscaller official github (we can clone it at anywhere)
<pre><code>
git clone https://github.com/kubernetes/autoscaler.git
cd /autoscaler/vertical-pod-autoscaler  # go inside directory
./hack/vpa-up.sh                        #run script
</code></pre>

vpa.yml
<pre><code>
kind: VerticalPodAutoscaler
apiVersion: autoscaling.k8s.io/v1
metadata:
  name: apache-vpa
  namespace: apache
spec:
  targetRef:
    name: apache-deployment
    apiVersion: apps/v1
    kind: Deployment
  updatePolicy:
    updateMode: "Auto"
</code></pre>

<pre><code>kubectl get vpa</code></pre>

