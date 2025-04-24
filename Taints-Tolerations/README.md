**Taints and Tolerations are powerful scheduling tools in Kubernetes. They control which pods can be scheduled on which nodes.**

What is a Taint?
A taint is applied to a node and tells Kubernetes:
- “Don’t schedule any pods here unless they can tolerate this taint.”

<pre><code>
#kubectl taint nodes node-name key=value:effect
</code></pre>

- key=value is a label-like pair.
- effect can be
- - NoSchedule → Only pods with a matching toleration will be scheduled
- - PreferNoSchedule → Tries to avoid scheduling, but not strict.
- - NoExecute → New pods are not scheduled and existing non-tolerating pods are evicted.

<pre><code>kubectl taint node cluster-worker pod=true:NoSchedule</code></pre>
<pre><code>kubectl taint node cluster-worker2 pod=true:NoSchedule</code></pre>
<pre><code>kubectl taint node cluster-worker3 pod=true:NoSchedule</code></pre>

![Alt-text](https://github.com/herrry107/Kubernetes/blob/main/images/taint.png)

What is a Toleration?
A toleration is added to a pod to say:
- “I can handle this taint, so it’s okay to put me on that node.”

<pre><code>
kind: Pod
apiVersion: v1
metadata:
  name: pod1
  namespace: nginx
spec:
  containers:
    - name: con1
      image: nginx:latest
      ports:
        - containerPort: 80
  tolerations:
    - key: "prod"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
</code></pre>

Remove Taint from node
<pre><code>kubectl taint node cluster-worker3 pod=true:NoSchedule-</code></pre>
