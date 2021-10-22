# Task: Deploy Node App on Kubernetes
The Nautilus development team has completed development of one of the node applications, which they are planning to deploy on a Kubernetes cluster. They recently had a meeting with the DevOps team to share their requirements. Based on that, the DevOps team has listed out the exact requirements to deploy the app. Find below more details:



Create a deployment using gcr.io/kodekloud/centos-ssh-enabled:node image, replica count must be 2.

Create a service to expose this app, the service type must be NodePort, targetPort must be 8080 and nodePort should be 30012.

Make sure all the pods are in Running state after the deployment.

You can check the application by clicking on NodeApp button on top bar.

You can use any labels as per your choice.


# Solution

This deployment is simple and can be done by `kubectl`:

    kubectl create deployment node --image gcr.io/kodekloud/centos-ssh-enabled:node --replicas 2 --port 8080

We can scaffold the service yaml and specify the `nodePort` manually:

    kubectl expose deployment node --type NodePort --port 8080 --target-port 8080 --dry-run=client -o yaml > svc.yml

Edit the svc.yml, add the nodePort and apply the svc.yml file:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node
spec:
  type: NodePort
  selector:
    app: node
  ports:
  - port: 8080
    targetPort: 8080
    nodePort: 30012
```
