# Task: Create Deployments in Kubernetes Cluster	
The Nautilus DevOps team has started practicing some pods, and services deployment on Kubernetes platform, as they are planning to migrate most of their applications on Kubernetes. Recently one of the team members has been assigned a task to create a deploymnt as per details mentioned below:



Create a deployment named httpd to deploy the application httpd using the image httpd:latest (remember to mention the tag as well)

# Solution

This deployment is so simple that we can create it with kubectl:

    kubectl create deployment httpd --image httpd:latest
