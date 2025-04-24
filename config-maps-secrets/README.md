In Kubernetes, a ConfigMap is used to store non-confidential configuration data as key-value pairs. This data can be injected into pods or other Kubernetes objects, allowing you to separate application configuration from the container image and manage it more dynamically.

configMap.yml
<pre><code>

kind: ConfigMap
apiVersion: v1
metadata:
  name: mysql-config-map
  namespace: mysql
data:
  MYSQL_DATABASE: devops

</code></pre>

<pre><code>kubectl apply -f configMap.yml</code></pre>

<pre><code>kubectl get configmap -n mysql</code></pre>

In Kubernetes, a Secret is an object used to store and manage sensitive information, such as:
- Passwords
- OAuth tokens
- SSH keys
- TLS certificates
- API keys

Using Secrets instead of storing confidential data in plain text in your Pod definitions or container images improves security.


**How Secrets Work**
Secrets are stored in etcd (Kubernetes' backing data store) and are base64-encoded, not encrypted by default (encryption at rest must be enabled explicitly).

We have to put string into base64 format 
<pre><code>
#convert root into base64 string 
echo "root" | base64
#output: cm9vdAo=
</code></pre>

secret.yml
<pre><code>

apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
  namespace: mysql
data:
  MYSQL_ROOT_PASSWORD: cm9vdAo=   #base64 encoded for root
</code></pre>

<pre><code>kubectl get secret -n mysql</code></pre>

statefulset.yml with configmap and secret
<pre><code>

kind: StatefulSet
apiVersion: apps/v1
metadata:
  name: mysql-statefulset
  namespace: mysql
spec:
  serviceName: mysql-service
  replicas: 3
  selector: 
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql  
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: MYSQL_ROOT_PASSWORD
            - name: MYSQL_DATABASE
              valueFrom: 
                configMapKeyRef:
                  name: mysql-config-map
                  key: MYSQL_DATABASE 
          volumeMounts:
            - name: mysql-data
              mountPath: /var/lib/mysql
  volumeClaimTemplates:   
    - metadata:
        name: mysql-data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi      
</code></pre>
