# Task: Deploy Nagios on Kubernetes	
The Nautilus DevOps team is planning to set up a Nagios monitoring tool to monitor some applications, services etc. They are planning to deploy it on Kubernetes cluster. Below you can find more details.



1) Create a deployment nagios-deployment for Nagios core. The container name must be nagios-container and it must use jasonrivers/nagios image.

2) Create a user and password for the Nagios core web interface, user must be xFusionCorp and password must be LQfKeWWxWD. (you can manually perform this step after deployment)

3) Create a service nagios-service for Nagios, which must be of targetPort type. nodePort must be 30008.

You can use any labels as per your choice.

# Solution

This solution is not elegant. Sensitive data such as user credentials should never be stored in plaintext. We could create a secret here `from literals` and mount it to the deployment and use the values for the environment variables instead of hardcoding them.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nagios-deployment
  name: nagios-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nagios-deployment
  template:
    metadata:
      labels:
        app: nagios-deployment
    spec:
      containers:
      - image: jasonrivers/nagios
        name: nagios-container
        env:
        - name: NAGIOSADMIN_USER
          value: xFusionCorp
        - name: NAGIOSADMIN_PASS
          value: LQfKeWWxWD
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: nagios-deployment
  name: nagios-service
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
    nodePort: 30008
  selector:
    app: nagios-deployment
  type: NodePort
```