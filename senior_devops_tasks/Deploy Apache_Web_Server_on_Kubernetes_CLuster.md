# Task: Deploy Apache Web Server on Kubernetes CLuster	

There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to deploy it. We need to develop a template as per requirements mentioned below:



Create a namespace named as httpd-namespace-nautilus.

Create a deployment named as httpd-deployment-nautilus under newly created namespace. For the deployment use httpd image with latest tag only and remember to mention the tag i.e httpd:latest, and make sure replica counts are 2.

Create a service named as httpd-service-nautilus under same namespace to expose the deployment, nodePort should be 30004.

# Solution

Simple resources can be created with kubectl:

    kubectl create ns httpd-namespace-nautilus

    kubectl -n httpd-namespace-nautilus create deploy httpd-deployment-nautilus --image httpd:latest --replicas 2

    kubectl -n httpd-namespace-nautilus expose deploy httpd-deployment-nautilus  --name httpd-service-nautilus --port 80 --target-port 80 --type NodePort

Under ports add `nodePort: 30004`:

    kubectl -n httpd-namespace-nautilus edit svc httpd-service-nautilus 