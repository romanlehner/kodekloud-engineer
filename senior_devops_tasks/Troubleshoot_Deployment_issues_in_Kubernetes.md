# Task: Troubleshoot Deployment issues in Kubernetes	

Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.



The deployment name is redis-deployment. The pods are not in running state right now, so please look into the issue and fix the same.

# Solution

Use `kubectl describe pod <pod-name>` to identify the problem. In this example the name of the configmap had a spelling error, as well as the redis image name.

Use `kubectl edit deployment redis-deployment` to correct the mistakes in the deployment.