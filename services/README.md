**SERVICES**

Kubernetes Services are a fundamental concept used to expose and manage access to applications running in a Kubernetes cluster. They provide a stable way to access Pods, which can come and go due to scaling or updates.

servicei.yml for create service

<pre><code>

kind: Service
apiVersion: v1
metadata:
  name: nginx-service
  namespace: nginx
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 80
  type: ClusterIP
</code></pre>


<pre><code>
#if cluster is created with kind than external forwarding command for docker nodes
kubectl port-forward service/nginx-service -n nginx 8080:8080 --address=0.0.0.0
</code></pre>
