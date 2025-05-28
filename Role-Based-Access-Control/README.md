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
