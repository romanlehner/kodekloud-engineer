# Task: Rolling Updates And Rolling Back Deployments in Kubernetes

There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.



Create a namespace devops. Create a deployment called httpd-deploy under this new namespace, It should have one container called httpd, use httpd:2.4.25 image and 2 replicas. The deployment should use RollingUpdate strategy with maxSurge=1, and maxUnavailable=2. Also create a NodePort type service named httpd-service and expose the deployment on nodePort: 30008.

Now upgrade the deployment to version httpd:2.4.43 using a rolling update.

Finally, once all pods are updated undo the recent update and roll back to the previous/original version.

# Solution

    kubectl create ns devops

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: httpd-deploy
  name: httpd-deploy
  namespace: devops
spec:
  replicas: 2
  selector:
    matchLabels:
      app: httpd-deploy
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  template:
    metadata:
      labels:
        app: httpd-deploy
    spec:
      containers:
      - image: httpd:2.4.25
        name: httpd
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: httpd-deploy
  name: httpd-service
  namespace: devops
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
    nodePort: 30008
  selector:
    app: httpd-deploy
  type: NodePort
```

    kubectl -n devops set image deploy httpd-deploy httpd=httpd:2.4.43
    
    kubectl -n devops rollout undo deploy httpd-deploy
