# Prerequisites

**Install Kubectl**

[Download and Install Kubectl from official site](https://kubernetes.io/docs/tasks/tools/)

<pre><code>
#check kubectl install or not
kubectl version
</code></pre>


**Install eksctl**

[Download and Install from eksctl github](https://github.com/eksctl-io/eksctl)

**or**

for unix/linux
<pre><code>
# for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"

# (Optional) Verify checksum
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check

tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz

sudo install -m 0755 /tmp/eksctl /usr/local/bin && rm /tmp/eksctl
</code></pre>

<pre><code>
#check eksctl install or not
eksctl version
</code></pre>

**Install awscli**

[Install awscli from official aws](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

**or**

for debian
<pre><code>
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
</code></pre>

<pre><code>
#check awscli install or not
aws version
</code></pre>

configure awscli for use
<pre><code>aws configure</code></pre>

