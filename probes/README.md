**In Kubernetes, a probe is a diagnostic mechanism used by the kubelet to determine the health or readiness of a container.**

There are three types of probes:

**1. Liveness Probe**
- Purpose: Checks if the application is still running.
- If it fails: Kubernetes restarts the container.
- Use case: Detecting and recovering from a deadlock or crash.

**2. Readiness Probe**
- Purpose: Checks if the application is ready to accept traffic.
- If it fails: The pod is removed from the service's endpoints.
- Use case: Preventing requests from going to a pod that isn't ready yet (e.g., still initializing or loading data).

**3. Startup Probe**
- Purpose: Used during container startup to give the app time before liveness probes are applied.
- If it fails: Container is killed and restarted.
- Use case: Long boot-up processes — prevents killing containers that just need more time.

**Probe Types**
Probes can be implemented using:
- HTTP GET: Calls a specific HTTP endpoint.
- TCP Socket: Checks if a TCP port is open.
- Exec Command: Runs a command inside the container.
