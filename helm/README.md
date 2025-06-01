# helm

Helm is a package manager for Kubernetes, like apt for Ubuntu or npm for Node.js. It simplifies deploying, managing, and upgrading Kubernetes applications by using something called Helm charts.

command to install helm
<pre><code>
curl -fsSL -o gett_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod +x gett_helm.sh
./gett_helm.sh
helm version
</code></pre>

create helm chart
<pre><code>helm create apache-helm</code></pre>

create package of helm chart after modified
<pre><code>
helm package apache-helm/
#output: Successfully packaged chart and saved it to: /root/Kubernetes/helm/apache-helm-0.1.0.tgz
</code></pre>

install helm-chart or run chart
<pre><code>helm install dev-apache apache-helm</code></pre>

uninstall created environment
<pre><code>helm uninstall dev-apache</code></pre>

install helm-chart or run chart in specific namespace
<pre><code>helm install dev-apache apache-helm -n dev-apache --create-namespace</code></pre>


