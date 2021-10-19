# Task: Troubleshoot Issue With Pods	
One of the junior DevOps team members was working on to deploy a stack on Kubernetes cluster. Somehow the pod is not coming up and its failing with some errors. We need to fix this as soon as possible. Please look into it.



There is a pod named webserver and the container under it is named as nginx-container. It is using image nginx:latest

There is a sidecar container as well named sidecar-container which is using ubuntu:latest image.

Look into the issue and fix it, make sure pod is in running state and you are able to access the app.

# Solution

Check the pod events with `kubectl describe pod webserver`. In this example the `nginx:latests` could not be pulled. As we can see there is a spelling mistake. The image name should be `nginx:latest`. 

As we cannot edit pods on the fly, we need to pull the pod's yaml config, delete the pod and apply the yaml config. Here the command for reference:

    kubectl get pod webserver -o yaml

Change the image name accordingly and remove unencessary lines until you can apply the yaml config.
