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

``` 
    git clone https://github.com/do-community/nodejs-image-demo.git node_project
```