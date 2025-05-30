# Setting Up the Kubernetes Dashboard

Deploy the Dashboard Apply the Kubernetes Dashboard manifest:

<pre><code>
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
</code></pre>

Create an Admin User Create a dashboard-admin-user.yml file with the following content:
<pre><code>
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user-binding
  namespace: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
</code></pre>

<pre><code>kubectl apply -f dashboard-admin-user.yml</code></pre>

Get the Access Token Retrieve the token for the admin-user:
<pre><code>kubectl -n kubernetes-dashboard create token admin-user</code></pre>

Copy the token for use in the Dashboard login.

Access the Dashboard Start the Dashboard using kubectl proxy:
<pre><code>kubectl proxy</code></pre>

Open the Dashboard in your browser:
<pre><code>http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/</code></pre>

Use the token from the previous step to log in.
