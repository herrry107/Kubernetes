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

In Kubernetes, a Secret is an object used to store and manage sensitive information, such as:
- Passwords
- OAuth tokens
- SSH keys
- TLS certificates
- API keys

Using Secrets instead of storing confidential data in plain text in your Pod definitions or container images improves security.


**How Secrets Work**
Secrets are stored in etcd (Kubernetes' backing data store) and are base64-encoded, not encrypted by default (encryption at rest must be enabled explicitly).

We have to put string into base64 format 
<pre><code>
#convert root into base64 string 
echo "root" | base64
</code></pre>

secret.yml
<pre><code>

apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
  namespace: mysql
data:
  MYSQL_ROOT_PASSWORD: cm9vdAo=   #base64 encoded for root
</code></pre>

<pre><code>kubectl get secret -n mysql</code></pre>
