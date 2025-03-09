### Introduction 

###### Implementing a reverse proxy with TLS/SSL on containers involves a different set of procedures from working directly on a host operating system. For example, if you were obtaining certificates from Let’s Encrypt for an application running on a server, you would install the required software directly on your host. Containers allow you to take a different approach. Using Docker Compose, you can create containers for your application, your web server, and the Certbot client that will enable you to obtain your certificates. By following these steps, you can take advantage of the modularity and portability of a containerized workflow.

### Prerequisite

##### In the following tutorial we wil need: 

- An Ubuntu 18.04 server, a non-root user with sudo privileges, and an active firewall. For guidance on how to set these up, please read this Initial Server Setup guide.

- Docker and Docker Compose installed on your server. For guidance on installing Docker, follow Steps 1 and 2 of How To Install and Use Docker on Ubuntu 18.04. For guidance on installing Compose, follow Step 1 of How To Install Docker Compose on Ubuntu 18.04.

- A registered domain name. This tutorial will use your_domain throughout. You can get one for free at Freenom, or use the domain registrar of your choice.

- Both of the following DNS records set up for your server. You can follow this introduction to DigitalOcean DNS for details on how to add them to a DigitalOcean account, if that’s what you’re using:

- An A record with your_domain pointing to your server’s public IP address.
- An A record with www.your_domain pointing to your server’s public IP address.
- Once you have everything set up, you’re ready to begin the first step.

__Step 1__ — Cloning and Testing the Node Application
As a first step, you’ll clone the repository with the Node application code, which includes the Dockerfile to build your application image with Compose. Then you’ll test the application by building and running it with the docker run command, without a reverse proxy or SSL. 

In your non-root user’s home directory, clone the nodejs-image-demo repository from the DigitalOcean Community GitHub account.

Clone the repository into a directory. This example uses node_project as the directory name. Feel free to name this directory to your liking:

``` bash
    git clone https://github.com/do-community/nodejs-image-demo.git node_project
```
Change into the node_project directory:
```bash
    cd node_project
```
In this directory, there is a Dockerfile that contains instructions for building a Node application using the Docker node:10 image and the contents of your current project directory. 

To test the application without SSL, you can build and tag the image using docker build and the -t flag. This example names the image node-demo, but you are free to name it something else:
```bash
    docker build -t node-demo .
```
Once the build process is complete, you can list your images with: 
```bash
    docker images
```

Next, create the container with docker run. Three flags are included with this command:

- -p: This publishes the port on the container and maps it to a port on your host. You will use port 80 on the host in this example, but feel free to modify this as necessary if you have another process running on that port. For more information about how this works, review this discussion in the Docker documentation on port binding.
- -d: This runs the container in the background.
- --name: This allows you to give the container a memorable name.

Run the following command to build the container:
```bash 
    docker run --name node-demo -p 80:8080 -d node-demo
```
Inspect your running containers with:
```bash
    docker ps 
```
You can now visit your domain to test your setup: http://your_domain. Remember to replace your_domain with your own domain name.

