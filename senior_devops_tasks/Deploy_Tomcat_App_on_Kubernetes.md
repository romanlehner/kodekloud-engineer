# Task: Deploy Tomcat App on Kubernetes	
A new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team to share the requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:



Create a namespace named tomcat-namespace-xfusion.

Create a deployment for tomcat app which should be named as tomcat-deployment-xfusion under the same namespace you created. Replica count should be 1, the container should be named as tomcat-container-xfusion, its image should be gcr.io/kodekloud/centos-ssh-enabled:tomcat and its container port should be 8080.

Create a service for tomcat app which should be named as tomcat-service-xfusion under the same namespace you created. Service type should be NodePort and nodePort should be 32227.

Before clicking on Check button please make sure the application is up and running.

You can use any labels as per your choice.

# Solution

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: tomcat-namespace-xfusion
  labels:
    app: tomcat-deployment-xfusion
  name: tomcat-deployment-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat-deployment-xfusion
  template:
    metadata:
      labels:
        app: tomcat-deployment-xfusion
    spec:
      containers:
      - image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
        name: tomcat-container-xfusion
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: tomcat-deployment-xfusion
  name: tomcat-service-xfusion
  namespace: tomcat-namespace-xfusion
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
    nodePort: 32227
  selector:
    app: tomcat-deployment-xfusion
  type: NodePort
```