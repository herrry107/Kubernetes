# Amazon EKS Namespace-Based Access Control

This guide demonstrates how to provide **read-only access** to a specific Kubernetes namespace (`dev`) using **EKS Access Entries**.

---

# Prerequisites

- Amazon EKS Cluster: `demo-cluster`
- IAM User: `test`
- IAM Role (for EC2): `eks-dev-team`

---

# 1. Grant `dev` Namespace Access to an IAM User

## Create an Access Entry (cluster admin run)

```bash
aws eks create-access-entry \
  --cluster-name demo-cluster \
  --principal-arn arn:aws:iam::577638353884:user/test
```

## Associate the View Policy (cluster admin run)

```bash
aws eks associate-access-policy \
  --cluster-name demo-cluster \
  --principal-arn arn:aws:iam::577638353884:user/test \
  --policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSViewPolicy \
  --access-scope type=namespace,namespaces=dev
```

This grants the IAM user **read-only** access to the `dev` namespace.

---

# 2. IAM Policy Required for the User

The IAM user also needs permission to describe the EKS cluster.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "eks:DescribeCluster"
      ],
      "Resource": "arn:aws:eks:ap-south-1:577638353884:cluster/demo-cluster"
    }
  ]
}
```

Attach this policy to the IAM user.

---

# 3. Connect to the Cluster (test user run)

Configure the kubeconfig for the user:

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name demo-cluster
```

---

# 4. Verify Read-Only Access (test user run)

Attempting to delete a pod should fail because the user only has **View** access.

```bash
kubectl delete pod dev-nginx -n dev
```

Output:

```text
Error from server (Forbidden): pods "dev-nginx" is forbidden:
User "arn:aws:iam::577638353884:user/test"
cannot delete resource "pods" in API group ""
in the namespace "dev"
```

---

# 5. Access EKS from an EC2 Instance (Without IAM User Credentials)

Instead of using an IAM user with access keys, attach an IAM Role to the EC2 instance.

## Step 1: Create an IAM Role

Create an IAM role (for example, `eks-dev-team`) with the following inline policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "eks:DescribeCluster"
      ],
      "Resource": "arn:aws:eks:ap-south-1:577638353884:cluster/demo-cluster"
    }
  ]
}
```

Attach the role to the EC2 instance.

---

## Step 2: Create an EKS Access Entry (cluster admin run)

Run the following command from an administrator machine:

```bash
aws eks create-access-entry \
  --cluster-name demo-cluster \
  --principal-arn arn:aws:iam::577638353884:role/eks-dev-team
```

---

## Step 3: Associate the Namespace Access Policy (cluster admin run)

```bash
aws eks associate-access-policy \
  --cluster-name demo-cluster \
  --principal-arn arn:aws:iam::577638353884:role/eks-dev-team \
  --policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSViewPolicy \
  --access-scope type=namespace,namespaces=dev
```

---

## Step 4: Update kubeconfig on the EC2 Instance (test user run)

Since the EC2 instance uses an IAM Role, no `aws configure` command is required.

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name demo-cluster
```

Verify access:

```bash
kubectl get pods -n dev
```

The role will have **read-only access** to the `dev` namespace.

---

# Summary

| Principal | IAM Permission | EKS Access Policy | Namespace |
|-----------|----------------|-------------------|-----------|
| IAM User (`test`) | `eks:DescribeCluster` | `AmazonEKSViewPolicy` | `dev` |
| IAM Role (`eks-dev-team`) | `eks:DescribeCluster` | `AmazonEKSViewPolicy` | `dev` |

Both the IAM user and the IAM role can:

- View resources in the `dev` namespace.
- Connect to the EKS cluster using `kubectl`.

They **cannot**:

- Create resources.
- Modify resources.
- Delete resources.
- Access other namespaces.
