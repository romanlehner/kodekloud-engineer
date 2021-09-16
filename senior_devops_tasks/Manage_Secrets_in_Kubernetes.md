# Task: Manage Secrets in Kubernetes

The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:



We already have a secret key file news.txt under /opt location on jump host. Create a generic secret named news, it should contain the password/license-number present in news.txt file.

Also create a pod named secret-devops.

Configure pod's spec as container name should be secret-container-devops, image should be debian preferably with latest tag (remember to mention the tag with image). Use sleep command for container so that it remains in running state. Consume the created secret and mount it under /opt/demo within the container.

To verify you can exec into the container secret-container-devops, to check the secret key under the mounted path /opt/demo. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

# Solution

Create secret from file:

    kubectl create secret generic news --from-file=/opt/news.txt

Create the Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-devops
spec:
  containers:
  - name: secret-container-devops
    image: debian:latest
    command: ['sh', '-c', 'while true; do sleep 100; done']
    volumeMounts:
    - name: news-secret
      mountPath: "/opt/demo"
      readOnly: true
  volumes:
  - name: news-secret
    secret:
      secretName: news
```