- **Kubernetes Namespace: A Kubernetes namespace is a way to divide cluster resources between multiple users or projects. Think of it like a virtual cluster within a real cluster.**

create or delete namespace
<pre><code>
#create namespace with name "nginx"
kubectl create ns nginx # command 1
kubectl create namespace nginx # command 2 we can use command 1 or 2, both are same

#delete namespace with name "nginx"
kubectl delete ns nginx # command 1
kubectl delete namespace nginx # command 2
</code></pre>

create namespace yml file

<pre><code>vim namespace.yml</code></pre>

<pre><code>
apiVersion: v1
kind: Namespace
metadata:
  name: nginx
</code></pre>

command to create namespace by yml file
<pre><code>kubectl apply -f namespace/namespace.yml</code></pre>

show all namespace
<pre><code>
kubectl get ns
</code></pre>

set any namespace to default
<pre><code>
#kubectl config set-context --current --namespace=
kubectl config set-context --current --namespace=nginx
</code></pre>
