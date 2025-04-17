# Kubernetes
Basic-to-Advance

KUBERNETES INSTALLATION ON UBUNTU LINUX

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

install helm 
<pre><code>
curl -fsSL -o gett_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
 chmod +x gett_helm.sh
 ./gett_helm.sh
 helm version
</code></pre>
 
