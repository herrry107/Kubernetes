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
              value: root
            - name: MYSQL_DATABASE
              value: devops
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

<pre><code>kubectl apply -f mysql-namespace.yml</code></pre>
<pre><code>kubectl apply -f mysql-service.yml</code></pre>
<pre><code>kubectl apply -f stateful-set.yml</code></pre>

![Alt-text](https://github.com/herrry107/Kubernetes/blob/main/images/mysql-statefull.png)

insert into container
<pre><code>kubectl exec -it mysql-statefulset-0 -n mysql -- bash</code></pre>
