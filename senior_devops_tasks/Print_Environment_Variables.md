# Task: Print Environment Variables

The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.



Create a pod named print-envars-greeting.

Configure spec as, the container name should be print-env-container and use bash image.

Create three environment variables:

a. GREETING and its value should be Welcome to

b. COMPANY and its value should be DevOps

c. GROUP and its value should be Datacenter

Use command to echo ["$(GREETING) $(COMPANY) $(GROUP)"] message.

You can check the output using <kubctl logs -f [ pod-name ]> command.

# Solution

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
  - name: print-env-container
    image: bash:latest
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "DevOps"
    - name: GROUP
      value: "Datacenter"
    command: ['sh', '-c', 'echo ["$(GREETING) $(COMPANY) $(GROUP)"]']
```