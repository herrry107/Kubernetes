# Istio

Istio is an open-source service mesh that provides a way to control how microservices share data with one another. It adds a layer of infrastructure between microservices and the network to manage service-to-service communication, security, and observability without requiring changes to your application code.

[istio documentation](https://istio.io/latest/docs/setup/getting-started/)

[Go to Istio installation page and select environment](https://istio.io/latest/docs/setup/install/)

# Setup Istio for kind-cluster

<pre><code>kind create cluster --name istio-testing</code></pre>

now download istion and run it
<pre><code>curl -L https://istio.io/downloadIstio | sh -</code></pre>
<pre><code>cd istio-1.26.1</code></pre>
<pre><code>mv istio/istio-1.26.1/bin/istioctl</code></pre>

Add a namespace label to instruct Istio to automatically inject Envoy sidecar proxies when you deploy your application later
<pre><code>kubectl label namespace default istio-injection=enabled</code></pre>



