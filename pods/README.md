**Kubernetes PODS**

**Kubernetes PODs: In Kubernetes, a pod is the smallest, deployable unit of computing and essentially encapsulates one or more application containers. Pods provide shared resources like networking, IP addresses, and storage (volumes) for these containers. They are ephemeral, meaning if a pod fails, Kubernetes can automatically recreate it.**

get running pods list
<pre><code>
#get pods in default nameserver
kubectl get pods
</code></pre>

<pre><code>
#get pods in specific nameserver
kubectl get pods -n nginx # nginx is nameserver name
</code></pre>

create pod in default namespace
<pre><code>
kubectl run pod1 --image=nginx
</code></pre>

create pod in specific namespace
<pre><code>
kubectl run pod2 --image=nginx -n nginx
</code></pre>

delete pod
<pre><code>
kubectl delete pod pod1
</code></pre>

delete pod in specific namespace
<pre><code>
kubectl delete pod pod2 -n nginx
</code></pre>

create pod file
<pre><code>
vim pods/pod1.yml
</code></pre>

<pre><code>
kind: Pod
apiVersion: v1
metadata: 
  name: pod1
  namespace: nginx
spec:
  containers:
    - name: con1
      image: nginx:latest
      ports:
        - containerPort: 80 
</code></pre>

create pod by apply command
<pre><code>
kubectl apply -f pods/pod1.yml
</code></pre>

Go inside active pod container
<pre><code>
kubectl exec -it pod/pod1 -n nginx -- bash
</code></pre>

Described info about running pod
<pre><code>kubectl describe pod/pod1 -n nginx</code></pre>

***ReplicaSet, StatefulSet and Deployment***

**REPLICASET**

- ***Purpose***: *Ensures a specified number of pod replicas are running at any given time.*

- ***Use Case***: *Low-level controller. Usually not used directly by users — instead, it's managed by a Deployment.*

- ***Behavior***:
    - *Only ensures pod count.*
    - *Doesn't provide rolling updates or versioning.*

**DEPLOYMENT**

- ***Purpose***: *Manages stateless applications and handles rolling updates, rollbacks, and scaling.*

- ***Use Case***: *The most common resource used to manage workloads.*

- ***Behavior***:
    - *Uses ReplicaSet under the hood.*
    - *Supports zero-downtime rolling updates.*
    - *Can rollback to a previous version.*
    - *Ideal for stateless applications.*

**STATEFULSET**
- ***Purpose***: *Manages stateful applications (those that require persistent storage and stable network identity).*

- ***Use Case***: *Databases, queues, and apps needing stable identity, ordered startup/shutdown, or persistent volume claims.*

- ***Behavior***:
    - *Gives each pod a stable hostname and persistent volume.*
    - *Pods are started/stopped in a specific order.*
    - *Not suitable for stateless apps.*
    - *Does not support rolling updates as smoothly as Deployment (may require manual steps).*

**Labels**
- *What: Key-value pairs attached to objects like Pods, Services, ReplicaSets, etc.*
- *Why: To organize, identify, and select subsets of objects.*

**Selectors**
- *What: Used to filter/select resources based on their labels.*
- *Why: So things like Services, ReplicaSets, Deployments can find the right pods to work with.*

deployment1.yml file for deployment demo
 
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
</code></pre>

<pre><code>kubectl apply -f deployment1.yml </code></pre>

<pre><code>kubectl get deployments -n nginx  #get info about deployment</code></pre>
<pre><code>
#scale up or down of replicas in existing deployment
kubectl scale deployment/nginx-deployment -n nginx --replicas=3
</code></pre>

<pre><code>
#update image of running deployment containers or change version to 1.27.3, firstly it will create another pods with running state after that it's delete previous version pod
kubectl set image deployment/nginx-deployment -n nginx=nginx:1.27.3
</code></pre>
<pre><code>kubectl delete -f pods/deployment1.yml</code></pre>

replicaset1.yml file for replicaSet demo
<pre><code>
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicasets
  namespace: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-rep-pod
  template:
    metadata:
      labels:
        app: nginx-rep-pod
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
</code></pre>

<pre><code>kubectl apply -f replicaSet1.yml</code></pre>
<pre><code>kubectl get replicasets -n nginx</code></pre>
<pre><code>
#scale up or down in existing replicasets
kubectl scale replicaset/nginx-replicasets -n nginx --replicas=3
</code></pre>
<pre><code>kubectl delete -f pods/replicaSet1.yml</code></pre>


