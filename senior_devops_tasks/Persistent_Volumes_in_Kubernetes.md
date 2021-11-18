# Task: Persistent Volumes in Kubernetes

The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


Create a PersistentVolume named as pv-devops. Configure the spec as storage class should be manual, set capacity to 4Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/security (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

Create a PersistentVolumeClaim named as pvc-devops. Configure the spec as storage class should be manual, request 1Gi of the storage, set access mode to ReadWriteOnce.

Create a pod named as pod-devops, mount the persistent volume you created with claim name pvc-devops at document root of the web server, the container within the pod should be named as container-devops using image httpd with latest tag only (remember to mention the tag i.e httpd:latest).

Create a node port type service named web-devops using node port 30008 to expose the web server running within the pod.

# Solution

**Note: By the time of solving this challenge, the hostpath of the volume was empty. I created my own index.html file just in case the system is evaluating the task based on the service response.

    echo 'Welcome to DatacenterCorp Industries!' > index.html
    kubectl cp index.html <pod>:/var/www/html
    
Here the yaml that need to be applied first:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-devops
spec:
  storageClassName: manual
  capacity:
    storage: 4Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/security"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-devops
spec:
  storageClassName: manual 
  volumeName: pv-devops
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-devops
  labels:
    app: httpd
spec:
  containers:
  - name: container-devops
    image: httpd:latest
    ports:
    - containerPort: 80
    volumeMounts:
    - name: pvc-devops
      mountPath: /var/www/html
  volumes:
  - name: pvc-devops
    persistentVolumeClaim:
      claimName: pvc-devops
---
apiVersion: v1
kind: Service
metadata: 
  name: web-devops
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:    
    app: httpd

```