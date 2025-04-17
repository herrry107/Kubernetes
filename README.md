# Kubernetes
Basic-to-Advance

k8s architecture

![Alt text](https://github.com/herrry107/Kubernetes/blob/main/K8-arch.png)

***KUBERNETES CLUSTER CREATION TYPE
- kubeadm
- minikube (local/ec2)
- kind cluster (docker)
- EKS/AKS/GKS

**KUBERNETES INSTALLATION ON UBUNTU LINUX**

bash command to setup kubernetes on ubuntu linux or aws

<pre><code>
#!/bin/bash
apt-get update
apt-get install apt-transport-https
apt install docker.io -y
systemctl start docker
systemctl enable docker
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg # allow unprivileged APT programs to read this keyring
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list
apt-get update
apt-get install kubectl -y
apt-get install -y kubeadm kubernetes-cni kubelet
</code></pre>

command to install helm 
<pre><code>
curl -fsSL -o gett_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod +x gett_helm.sh
./gett_helm.sh
helm version
</code></pre>

**Kubernetes Kind: It’s a tool for running local Kubernetes clusters using Docker containers as "nodes"**

command to setup kubernetes kind 
<pre><code>
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
apt-get install docker.io –y

</code></pre>

***Kubernetes Config file to create control-plane and nodes into docker***
<pre><code>
mkdir kind-cluster
cd kind-cluster
touch kind-config.yml
</code></pre>


kind-cluster/kind-config.yml
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

get nodes
<pre><code>kubectl get nodes</code></pre>

change kubectl default cluster
<pre><code>kubectl config use-context kind-cluster1</code></pre>

**Kubernetes Namespace: A Kubernetes namespace is a way to divide cluster resources between multiple users or projects. Think of it like a virtual cluster within a real cluster.**

create or delete namespace
<pre><code>
#create namespace with name "nginx"
kubectl create ns nginx # command 1
kubectl create namespace nginx # command 2 we can use command 1 or 2, both are same

#delete namespace with name "nginx"
kubectl delete ns nginx # command 1
kubectl delete namespace nginx # command 2
</code></pre>

create namespace dir 
<pre><code>
mkdir namespace
cd namespace
touch namespace.yml
</code></pre>

create namespace yml file
<pre><code>
apiVersion: v1
kind: Namespace
metadata:
  name: nginx
</code></pre>


command to create namespace by yml file 
<pre><code>kubectl apply -f namespace/namespace.yml</code></pre>

set any namespace to default
<pre><code>
#kubectl config set-context --current --namespace=<your-namespace>
kubectl config set-context --current --namespace=nginx
</code></pre>

