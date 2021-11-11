# Task: Fix Python App Deployed on Kubernetes Cluster
One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.



The deployment name is python-deployment-devops, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.

nodePort should be 32345 and targetPort should be python flask app's default port.

# Solution

- Describe the failing pod. In my case the image name was wrong and changed it accordingly:

        kubectl set image deploy python-deployment-devops python-container-devops=poroko/flask-demo-app


- Check the logs of the app to get the default port of the flask application. In my case it was port `5000`.
- Check the service resource. In my case the target port was set to `8080` but should be `5000`. I used `kubectl edit svc <svc-name>` to make the adjustment.
