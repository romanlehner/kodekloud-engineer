# Task: Kubernetes Shared Volumes	
We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.



Create a pod named volume-share-devops.

For the first container, use image centos with latest tag only and remember to mention the tag i.e centos:latest, container should be named as volume-container-devops-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/beta.

For the second container, use image centos with the latest tag only and remember to mention the tag i.e centos:latest, container should be named as volume-container-devops-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/demo.

Volume name should be volume-share of type emptyDir.

After creating the pod, exec into the first container i.e volume-container-devops-1, and just for testing create a file beta.txt with any content under the mounted path of first container i.e /tmp/beta.

The file beta.txt should be present under the mounted path /tmp/demo on the second container volume-container-devops-2 as well, since they are using a shared volume.

# Solution

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  containers:
  - name: volume-container-devops-1
    image: centos:latest
    command: ['sh', '-c', 'while true; do sleep 1; done']
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/beta
  
  - name: volume-container-devops-2
    image: centos:latest
    command: ['sh', '-c', 'while true; do sleep 1; done']
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/demo

  volumes:
  - name: volume-share
    emptyDir: {}

```

Exec into the first container and create a file in the specified shared volume directory:

    kubectl exec -it volume-share-devops -c volume-container-devops-1 -- touch /tmp/beta/beta.txt

Check if the file is present in the other container volume mount path:

    kubectl exec -it volume-share-devops -c volume-container-devops-2 -- ls -la /tmp/demo
