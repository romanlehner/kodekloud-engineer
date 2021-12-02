# Task: Create a Linux User with non-interactive shell

The System admin team of xFusionCorp Industries has installed a backup agent tool on all app servers. As per the tool's requirements they need to create a user with a non-interactive shell.


Therefore, create a user named siva with a non-interactive shell on the App Server 3

# Solution

Add the user:

    adduser siva  -s /sbin/nologin

Check the user has been added:

    id siva
    cat /etc/passwd | grep siva
