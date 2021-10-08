# Task: Deploy Lamp Stack on Kubernetes Cluster	
The Nautilus DevOps team want to deploy a PHP website on Kubernetes cluster. They are going to use Apache as a web server and Mysql for database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:



1) Create a config map php-config for php.ini with variables_order = "EGPCS" data.

2) Create a deployment named lamp-wp.

3) Create two containers under it. First container must be httpd-php-container using image webdevops/php-apache:alpine-3-php7 and second container must be mysql-container from image mysql:5.6. Mount php-config configmap in httpd container at /opt/docker/etc/php/php.ini location.

4) Create kubernetes generic secrets for mysql related values like myql root password, mysql user, mysql password, mysql host and mysql database. Set any values of your choice.

5) Add some environment variables for both containers:

a) MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD and MYSQL_HOST. Take their values from the secrets you created.

6) Create a node port type service lamp-service to expose the web application, nodePort must be 30008.

7) Create a service for mysql named mysql-service and its port must be 3306.

8) We already have /tmp/index.php file on jump_host server.

a) Copy this file into httpd container under Apache document root i.e /app and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.

b) You must be able to access this index.php on node port 30008 at the end, please note that you should see Connected successfully message while accessing this page.

# Solution

Note: The most efficient way to add the env variables would be:

```yaml
envFrom:
- secretRef:
  name: <your_secret_name>
```

At the current time of writing, the task validation looked for `env` definitions in the yaml file and doesn't allow this approach. We have to define each environment variable seperately:

```yaml
- name: SECRET_USERNAME
  valueFrom:
    secretKeyRef:
      name: mysecret
      key: username
```

Create the generic secret:

    kubectl create secret generic db --from-literal=MYSQL_ROOT_PASSWORD=pass --from-literal=MYSQL_USER=user --from-literal=MYSQL_PASSWORD=pass --from-literal=MYSQL_HOST=mysql-service --from-literal=MYSQL_DATABASE=db

Create the other resources:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: php-config
data:
  php.ini: | 
    variables_order="EGPCS"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lamp-wp
  labels:
    app: lamp
spec:
  selector:
    matchLabels:
      app: lamp
  template:
    metadata:
      labels:
        app: lamp
    spec:
      containers:
      - name: httpd-php-container
        image: webdevops/php-apache:alpine-3-php7 
        ports:
        - containerPort: 80
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_USER
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_PASSWORD
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_HOST
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_DATABASE
        volumeMounts:
        - mountPath: /opt/docker/etc/php/php.ini
          subPath: php.ini
          name: php-config
      - name: mysql-container 
        image: mysql:5.6
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_USER
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_PASSWORD
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_HOST
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: db
              key: MYSQL_DATABASE
      volumes:
      - name: php-config
        configMap:
          name: php-config
---
apiVersion: v1
kind: Service
metadata:
  name: lamp-service  
spec:
  selector:
    app: lamp
  type: NodePort
  ports:
  - protocol: TCP 
    port: 80
    targetPort: 80
    nodePort: 30008
---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service  
spec:
  selector:
    app: lamp
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
```


Modify the `/tmp/index.php` file accordingly and copy into the container:

```
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];
```

    kubectl cp -c httpd-php-container /tmp/index.php <pod>:/app/index.php 
