# Task: Fix Issue with VolumeMounts in Kubernetes

We deployed a Nginx and PHPFPM based setup on Kubernetes cluster last week and it had been working fine. This morning one of the team members made a change somewhere which caused some issues, and it stopped working. Please look into the issue and fix it:

The pod name is nginx-phpfpm and configmap name is nginx-config. Figure out the issue and fix the same.

Once issue is fixed, copy /home/thor/index.php file from jump host into nginx-container under nginx document root and you should be able to access the website using Website button on top bar.

# Solution

Describe the pod and check the volume mounts as well as the config map. The config map shows that the root document is configured to be `/var/www/html`. In the pod one of the containers has a different path configured as `mountPath`.

Pods need to be destroyed and recreated to accept changes. First extract the current config:

    kubectl get pod nginx-phpfpm -o yaml > pod.yaml
    kubectl delete pod nginx-phpfpm

Modify the yaml file accordingly and create the new pod and copy the index.php file to the required container directory:

    kubectl apply -f pod.yaml

    kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container

Clicking the website button should display a working page.