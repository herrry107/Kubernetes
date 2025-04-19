**JOBS**

A Job in Kubernetes is used to run one-off, short-lived, or batch tasks—ensuring they complete successfully at least once.
Unlike Deployments or DaemonSets (which run indefinitely), a Job runs until a task completes.


**Basic Job Behavior**
- A Job creates one or more Pods and waits for them to complete successfully.
- If a Pod fails, it can retry (based on backoffLimit).
- A Job is complete when the specified number of successful completions is reached (completions field).
- Once complete, the Job’s Pods exit (they don’t restart like in Deployments).

job1.yml

<pre><code>
kind: Job
apiVersion: batch/v1
metadata:
  name: demo-job
  namespace: nginx
spec:
  completions: 1
  parallelism: 1
  template:
    metadata:
      name: demo-job-pod
      labels:
        app: batch-task
    spec:
      containers:
      - name: batch-container
        image: busybox:latest
        command: ["sh","-c","echo hello dosto! && sleep 10"]
      restartPolicy: Never     
</code></pre>

<pre><code>kubectl get jobs -n nginx</code></pre>
<pre><code>kubectl get pods -n nginx</code></pre> 
<pre><code>kubectl logs pod/demo-job-m5szw -n nginx</code></pre>

**CRONJOB**

**A CronJob is a scheduled job in Unix-like operating systems, used to run scripts or commands automatically at specified times and intervals. It is defined in the crontab (cron table), and the format looks like this:**

![Alt text](https://github.com/herrry107/Kubernetes/blob/main/jobs/cron.png)

cronjob1.yml for cronjob demo
<pre><code>
apiVersion: batch/v1
kind: CronJob
metadata:
  name: minute-backup
  namespace: nginx
spec:
  schedule: "* * * * * "
  jobTemplate:
    spec:
      template:
        metadata:
          name: minute-backup
          labels:
            app: minute-backup
        spec:
          containers:
            - name: backup-container
              image: busybox
              command:
                - sh
                - -c
                - >
                  echo "Backup Started";
                  mkdir -p /backups &&
                  mkdir -p /data &&
                  cp -r /demo-data /backups &&
                  echo "Backup Completed";
              volumeMounts:
                - name: data-volume
                  mountPath: /demo-data
                - name: backup-volume
                  mountPath: /backups
          restartPolicy: OnFailure
          volumes:
            - name: data-volume
              hostPath:
                path: /demo-data
                type: DirectoryOrCreate
            - name: backup-volume
              hostPath:
                path: /backups
                type: DirectoryOrCreate

</code></pre>

<pre><code>kubectl get cronjob -n nginx</code></pre>
