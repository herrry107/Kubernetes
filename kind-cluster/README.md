**Kubernetes Kind**

**Kubernetes Kind: It’s a tool for running local Kubernetes clusters using Docker containers as "nodes"**

kind cluster create script
<pre><code>
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
  - role: control-plane
    image: kindest/node:v1.31.2
  - role: worker
    image: kindest/node:v1.31.2  
  - role: worker
    image: kindest/node:v1.31.2
  - role: worker
    image: kindest/node:v1.31.2
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP

</code></pre>

add current user into docker group or create new group
<pre><code>sudo usermod -aG docker $USER && newgrp docker</code></pre>

create kind cluster
<pre><code>kind create cluster --name=cluster1 --config=kind-cluster/kind-config.yml</code></pre>

get info about kind cluster
<pre><code>kubectl cluster-info --context kind-cluster1</code></pre>

delete kind cluster
<pre><code>kind delete cluster --name=cluster1</code></pre>

change kubectl default cluster
<pre><code>kubectl config use-context kind-cluster1</code></pre>

get nodes 
<pre><code>kubectl get nodes</code></pre>
