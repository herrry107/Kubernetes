In Kubernetes, a ConfigMap is used to store non-confidential configuration data as key-value pairs. This data can be injected into pods or other Kubernetes objects, allowing you to separate application configuration from the container image and manage it more dynamically.

<pre><code>

kind: ConfigMap
apiVersion: v1
metadata:
  name: mysql-config-map
  namespace: mysql
data:
  MYSQL_DATABASE: devops

</code></pre>

<pre><code>kubectl apply -f configMap.yml</code></pre>

<pre><code>kubectl get configmap -n mysql</code></pre>
