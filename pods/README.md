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
