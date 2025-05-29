# RBAC ( Role Based Access Control)

![RBAC](https://github.com/herrry107/Kubernetes/blob/main/images/RBAC.png)

Role-Based Access Control (RBAC) in Kubernetes is a method for regulating access to Kubernetes resources based on the roles of individual users or service accounts within your cluster. It lets you define who can do what on which resources.

**Key Concepts in Kubernetes RBAC**

***Role:***
- Defines a set of permissions (rules) within a namespace.
- Example: A Role that allows reading Pods in the dev namespace.

***ClusterRole:***
- Similar to Role, but used for cluster-wide permissions or across all namespaces.
- Can be used to grant access to non-namespaced resources (like nodes).

***RoleBinding:***
- Grants the permissions defined in a Role to a user or group within a specific namespace.

***ClusterRoleBinding:***
- Grants the permissions defined in a ClusterRole to a user or group cluster-wide.

The **kubectl auth** command in Kubernetes is used to inspect and verify authorization. It helps you determine whether a user, group, or service account is authorized to perform a specific action on a Kubernetes resource.

know auth user
<pre><code>
#now we are admin
kubectl auth whoami
#output:
#    ATTRIBUTE   VALUE
#    Username    kubernetes-admin
#    Groups      [kubeadm:cluster-admins system:authenticated]
</code></pre>

check permission to run get command
<pre><code>
kubectl auth can-i get pods -n nginx
#output: yes
</code></pre>

check permission to run get command
<pre><code>
kubectl auth can-i get deployment -n nginx
#output: yes
</code></pre>

but we don't want to all user access all namespace aur specific command

first we will create role.yml file

role.yml
<pre><code>
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: apache-manager
  namespace: apache
rules:
  - apiGroups: ["apps","rbac.authorization.k8s.io/v1"]        # "" blank group mean for all
    resources: ["deployment","pod","service"]                 # access for resources allow
    verbs: ["get","apply","delete","watch","create","patch"]  # access for commands use
</code></pre>

<pre><code>kubectl apply -f role.yml</code></pre>

<pre><code>
kubectl get role -n apache
#   output:
#       NAME             CREATED AT
#       apache-manager   2025-05-29T17:17:45Z
</code></pre>

now we will create serviceAccount, serviceAccount is like special user 

service-account.yml
<pre><code>
kind: ServiceAccount
apiVersion: v1
metadata: 
  name: apache-user
  namespace: apache
</code></pre>

<pre><code>kubectl apply -f service-account.yml</code></pre>

<pre><code>
kubectl get serviceaccount -n apache
#   output:
#       NAME          SECRETS   AGE
#       apache-user   0         7m13s
#       default       0         10m
</code></pre>

now we will create role-binding

role.binding.yml
<pre><code>
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: apache-manager-rolebinding
  namespace: apache

subjects:
- kind: User
  name: apache-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: apache-manager
  apiGroup: rbac.authorization.k8s.io
</code></pre>

<pre><code>kubectl get rolebinding -n apache</code></pre>

<pre><code>kubectl auth can-i get pods --as=apache-user -n apache</code></pre>


