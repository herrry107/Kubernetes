# Kubernetes
**Kubernetes is an open-source container orchestration platform. It automates the deployment, management, and scaling of containerized applications. Think of it as a system that helps you manage and organize your applications when they are running in containers, especially when you have multiple containers and need to distribute them across different physical or virtual machines.**

Basic-to-Advance

k8s architecture

![Alt text](https://github.com/herrry107/Kubernetes/blob/main/K8-arch.png)

KUBERNETES CLUSTER CREATION TYPE
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

create NAMESPACE folder 
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

show all namespace
<pre><code>kubectl get ns</code></pre>

set any namespace to default
<pre><code>
#kubectl config set-context --current --namespace=<your-namespace>
kubectl config set-context --current --namespace=nginx
</code></pre>

- **Kubernetes PODs: In Kubernetes, a pod is the smallest, deployable unit of computing and essentially encapsulates one or more application containers. Pods provide shared resources like networking, IP addresses, and storage (volumes) for these containers. They are ephemeral, meaning if a pod fails, Kubernetes can automatically recreate it.**

get running pods list

<pre><code>#get pods in default nameserver
kubectl get pods</code></pre>

<pre><code>#get pods in specific nameserver
kubectl get pods -n nginx # nginx is nameserver name
</code></pre>

create pod in default namespace
<pre><code>kubectl run pod1 --image=nginx</code></pre>

create pod in specific namespace
<pre><code>kubectl run pod2 --image=nginx -n nginx</code></pre>

delete pod 
<pre><code>kubectl delete pod pod1</code></pre>

delete pod in specific namespace
<pre><code>kubectl delete pod pod2 -n nginx</code></pre>

create POD folder
<pre><code>
mkdir pods
touch pods/pod1.yml
</code></pre>

create pod by yml file

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
<pre><code>kubectl apply -f pods/pod1.yml</code></pre>

Go inside active pod container
<pre><code>kubectl exec -it pod/pod1 -n nginx -- bash</code></pre>

**KUBECTL: kubectl is the primary command-line tool for interacting with Kubernetes clusters. It's used to deploy applications, manage resources, view logs, and perform other operations on a Kubernetes cluster. Essentially, it's the way you tell Kubernetes what to do.**

<pre><code>
kubectl get pods            # get pods list
kubectl get nodes           # get nodes 
kubectl get ns              # get namespace
kubectl get pods -n nginx   # get pod in specific namespace
