# Kubernetes
**Kubernetes is an open-source container orchestration platform. It automates the deployment, management, and scaling of containerized applications. Think of it as a system that helps you manage and organize your applications when they are running in containers, especially when you have multiple containers and need to distribute them across different physical or virtual machines.**

Basic-to-Advance

k8s architecture

![Alt text](https://github.com/herrry107/Kubernetes/blob/main/images/K8-arch.png)

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
apt install curl -y
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

**KUBECTL: kubectl is the primary command-line tool for interacting with Kubernetes clusters. It's used to deploy applications, manage resources, view logs, and perform other operations on a Kubernetes cluster. Essentially, it's the way you tell Kubernetes what to do.**

<pre><code>
kubectl get pods            # get pods list
kubectl get nodes           # get nodes 
kubectl get ns              # get namespace
kubectl get pods -n nginx   # get pod in specific namespace
</code></pre>
![Alt text](https://github.com/herrry107/Kubernetes/blob/main/images/deployment-service.png)

# Read All Kubernetes Docs by these sequence

- [1. Create Kind-Cluster](https://github.com/herrry107/Kubernetes/tree/main/kind-cluster)
- [2. Create Minikube](https://github.com/herrry107/Kubernetes/tree/main/minikube)
- [3. Namespace](https://github.com/herrry107/Kubernetes/tree/main/namespace)
- [4. Pods](https://github.com/herrry107/Kubernetes/tree/main/pods)
- [5. Jobs](https://github.com/herrry107/Kubernetes/tree/main/jobs)
- [6. Volumes](https://github.com/herrry107/Kubernetes/tree/main/volume)
- [7. Services](https://github.com/herrry107/Kubernetes/tree/main/services)
- [8. Ingress](https://github.com/herrry107/Kubernetes/tree/main/ingress)
- [9. ConfigMaps/Secrets](https://github.com/herrry107/Kubernetes/tree/main/config-maps-secrets)
- [10. Probes](https://github.com/herrry107/Kubernetes/tree/main/probes)
- [11. Taints/Tolerations](https://github.com/herrry107/Kubernetes/tree/main/Taints-Tolerations)
- [12. Auto Scalling](https://github.com/herrry107/Kubernetes/tree/main/AutoScalling)
- [13. Role Based Access Control](https://github.com/herrry107/Kubernetes/tree/main/Role-Based-Access-Control)
- [14. Monitoring and Logging](https://github.com/herrry107/Kubernetes/tree/main/monitoring-and-logging)


 
