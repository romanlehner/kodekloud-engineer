# Task: Create a Docker Image From Container	

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:



a. Create an image beta:nautilus on Application Server 2 from a container ubuntu_latest that is running on same server.

# Solution

The task suggests that the container might be modified compared to its base image. We create an image from it by commiting its current state:

    docker commit <container_id> beta:nautilus