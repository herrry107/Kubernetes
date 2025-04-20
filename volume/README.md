**Persistent Volume (PV) and Persistent Volume Claim (PVC) are essential components of the storage system that allow pods to use storage resources independently of the underlying physical storage. Here's a clear breakdown of what they are and how they work together**

**Persistent Volume (PV)**
A Persistent Volume is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using a StorageClass. It’s a resource in the cluster, just like a node is a cluster resource.

*Key points about PVs:*
- Defined and managed by the cluster.
- Can use different storage backends (e.g., NFS, iSCSI, AWS EBS, GCE Persistent Disk, etc.).
- Lifecycle is independent of any pod using it.
- Created manually or dynamically.


**Persistent Volume Claim (PVC)**
- A Persistent Volume Claim is a request for storage by a user. It specifies:
- How much storage is needed.
- What access mode is required (e.g., read/write).
- (Optionally) Which StorageClass to use.

*Key points about PVCs:*
- Users don’t need to know the details of the underlying storage.
- Kubernetes finds a matching PV (if available) and binds it to the PVC.
- PVCs are used in pod specs to mount the volume.

persistent-volume.yml for persistentvolume demo

<pre><code>
kind: PersistentVolume
apiVersion: v1
metadata:
  name: local-pv
  labels:
    app: local
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: local-storage
  hostPath:
    path: /mnt/data
</code></pre>

<pre><code>kubectl get pv</code></pre>

persistent-volume-claim.yml for persistent volume claim demo
<pre><code>

kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: local-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: local-storage    
</code></pre>

<pre><code>kubectl get pvc</code></pre>  
