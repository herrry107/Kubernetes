# Custom Resource Definition

A Custom Resource Definition (CRD) allows you to define your own Kubernetes resource types. Once a CRD is applied to the cluster, you can create and manage resources of that new type using kubectl, just like any built-in resource.

custom-resource-definition.yml
<pre><code>
kind: CustomResourceDefinition
apiVersion: apiextensions.k8s.io/v1
metadata:
  name: devopsbatches.pratikdevops.com
spec:
  group: pratikdevops.com
  names:
    plural: devopsbatches
    singular: devopsbatch
    kind: DevOpsBatch
    shortNames:
      - junoon
      - batches
      - pratik
  scope: Namespaced
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec: 
            type: object
            properties: 
              name:
                type: string
                description: This is the name of devops batch
              duration:
                type: string
                description: "This is the duration of devops batch"
              mode:
                type: string
                description: "online classes"
              platform:
                type: string
                description: "This is the description of platform"    
</code></pre>

<pre><code>kubectl apply -f custom-resource-definition.yml</code></pre>

<pre><code>kubectl get crd</code></pre>

now we can create our custom resource

devops-cr.yml
<pre><code>
kind: DevOpsBatch
apiVersion: pratikdevops.com/v1
metadata:
  name: junnon-batch-9
spec:
  name: Devops-batch1
  duration: 3 month
  mode: live as always
  platform: pratik
</code></pre>

<pre><code>
kubectl get pratik
#output
# NAME             AGE
# junnon-batch-9   84s

