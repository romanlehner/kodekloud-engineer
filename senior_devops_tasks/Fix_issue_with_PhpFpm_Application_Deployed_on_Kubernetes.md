# Task: Fix issue with PhpFpm Application Deployed on Kubernetes
We deployed a Nginx and PHPFPM based application on Kubernetes cluster last week and it had been working fine. This morning one of the team members was troubleshooting an issue with this stack and he was supposed to run Nginx welcome page for now on this stack till issue with phpfpm is fixed but he made a change somewhere which caused some issue and the application stopped working. Please look into the issue and fix the same:



The deployment name is nginx-phpfpm-dp and service name is nginx-service. Figure out the issues and fix them. FYI Nginx is configured to use default http port, node port is 30008 and copy index.php under /tmp/index.php to deployment under /var/www/html. Please do not try to delete/modify any other existing components like deployment name, service name etc.

# Solution

1. Check the nginx listener config in the config map
1. Make sure the `targetPort` in the service resource is set to the nignx listener port.
1. Check the config map for errors (spelling mistake `index.ph p` should be `index.php`)
1. Reload the deployment `kubectl rollout restart deployment nginx-phpfpm-dp`
1. Copy the index.php file into the pod `kubectl cp /tmp/index.php <pod>:/var/www/html`
1. Confirm the application works as expected from the kodekloud interface on port `30008`