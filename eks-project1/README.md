# Basic EKS Cluster Project

**we will learn**

- EKS cluster
- Service (Load Balancer)
- Ingress Controller (nginx)

**Prerequisites**

- kubectl: A command line tool for working with Kubernetes clusters. For more information, see Installing or updating kubectl.

- eksctl: A command line tool for working with EKS clusters that automates many individual tasks. For more information, see Installing or updating.

- AWS CLI: A command line tool for working with AWS services, including Amazon EKS. For more information, see Installing, updating, and uninstalling the AWS CLI in the AWS Command Line Interface User Guide. After installing the AWS CLI, we recommend that you also configure it. For more information, see Quick configuration with aws configure in the AWS Command Line Interface User Guide.

[Install kubectl/eksctl/aws-cli](https://github.com/herrry107/Kubernetes/blob/main/eks-project1/prerequisites.md)

# EKS Cluster

create eks cluster
<pre><code>
eksctl create cluster --name demo-cluster --region ap-south-1 --fargate
</code></pre>

delete eks cluster
<pre><code>
eksctl delete cluster --name demo-cluster --region ap-south-1 --fargate
</code></pre>

configures your local kubectl to connect to your AWS EKS (Elastic Kubernetes Service) cluster named demo-cluster in the ap-south-1 region
<pre><code>
aws eks update-kubeconfig --name demo-cluster --region ap-south-1
</code></pre>

# Create Fargate Profile
<pre><code>
eksctl create fargateprofile \
    --cluster demo-cluster \
    --region ap-south-1 \
    --name alb-sample-app \
    --namespace game-2048
</code></pre>

This command creates a Fargate profile in your Amazon EKS cluster named demo-cluster, allowing pods in the namespace game-2048 to run on AWS Fargate, which is a serverless compute engine for containers.

# Deploy the deployment, service and Ingress

**depoyment-service-ingress.yml**
<pre><code>
---
apiVersion: v1
kind: Namespace
metadata:
  name: game-2048
---
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: game-2048
  name: deployment-2048
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: app-2048
  replicas: 5
  template:
    metadata:
      labels:
        app.kubernetes.io/name: app-2048
    spec:
      containers:
      - image: public.ecr.aws/l6m2t8p7/docker-2048:latest
        imagePullPolicy: Always
        name: app-2048
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  namespace: game-2048
  name: service-2048
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  type: NodePort
  selector:
    app.kubernetes.io/name: app-2048
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  namespace: game-2048
  name: ingress-2048
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: service-2048
              port:
                number: 80
</code></pre>

<pre><code>kubectl apply -f depoyment-service-ingress.yml</code></pre>
<pre><code>kubectl get all -n game-2048</code></pre>

currently we don't have ingress controller that's  why we are not showing  address in ingress

first we need to create IAM OIDC provider because alb load balancer which is running need to connect application load balancer

# commands to configure IAM OIDC provider

<pre><code>export cluster_name=demo-cluster</code></pre>

<pre><code>oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5) </code></pre>

Check if there is an IAM OIDC provider configured already

<pre><code>eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve</code></pre>

if above command doesn't run then
<pre><code>aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "/" -f4\n</code></pre>

#  How to setup alb add on

Download IAM policy
<pre><code>
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
</code></pre>

