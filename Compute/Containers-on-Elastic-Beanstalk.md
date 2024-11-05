# Containers on Elastic Beanstalk

In this project, I will use AWS Elastic Beanstalk to deploy a containerised application. To create a container, I will be using Docker.

## Set up an IAM User

To get started, I had to create an IAM user.

To do this, I simply created a new IAM user with Administrator Permissions, from my root account.

## Docker

### What is Docker?

Docker allows us to containerise an application so we can develop, ship and deploy an application in an isolated environment.

### What is a container?

Containers package an application and everything it needs to run in one file. With this, others are able to run this package instead of the application itself with all the necessary dependencies instead of manually downloading anything that is required to run the application.

### What is Docker Daemon?

Docker daemon is the underlying process that manages all the containers and handles all the processes for the containers, on a device.

## Test

To test Docker, I created a new Image using the template repository, provided by Docker, and ran it using the Docker app. Here, the Docker Daemon has checked all the dependencies and built the app using the requirementes specified through the dockerfile and the dockerignore file.

Next, I ran the command `docker run -d -p 80:80 nginx`.

- `docker run` starts a new container.
- `nginx` is a pre-existing container image
- `-d` starts this container in detached mode (runs in the background)
- `-p 80:80` maps port 80 on the host machine to port 80 on the container
  - Which allows us access to the webpage (on nginx) through the computer's browser.

### What is Nginx?

Nginx is a web server, which means it's a program that serves web pages to people on the internet. Nginx can handle lots of web traffic smoothly and efficiently. It is commonly referred to as a 'proxy server', which means it can also be used to forward requests from the internet to other servers, helping balance the load or handle more users.

### What is a container Image?

A container image is a template used to create containers. It contains the application code, libraries, dependencies, and other files needed to run the application. One function of images is that it allows users to make multiple, identical containers from the same image, and all those containers will behave the same regardless of where it's deployed.

### What is the difference between a container and a container image?

A container image is a read-only template that conatins instructions for creating containers (akin to a snapshot or an image) that can be stored in registries (e.g. docker hub) and can be used to create multiple containers.  
Whereas, a container is a running instance of an image with the actual execution environment and has read-write capabilities. It can be started, stopped, moved, or deleted and has its own state and running processes.

### Opening http://localhost

Since I've mapped port 80 of my machine to port 80 of the container that's running, the server software that localhost is connected to is the Nginx server inside the container. Web browsers automatically assume you're referring to port 80 when you don't specify a port number. So routing to `http://localhost` is basically the same as routing to `http://localhost:80`.

## Next step: Create custom image.

> ### Steps:
>
> - Create file called **Compute** with `dockerfile` inside
> - Add the following lines to `dockerfile`:
>
> > ```dockerfile
> > FROM nginx:latest
> > COPY index.html /usr/share/nginx/html/
> > EXPOSE 80
> > ```
>
> - Create index.html file with the contents provided by NextWork

### What do these lines mean?

`FROM nginx:latest`: Image starts as a copy of the latest Nginx image.  
`COPY index.html /usr/share/nginx/html/`: replaces the HTML provided by Nginx with your custom `index.html` file. Basically, I'm customizing the Nginx server to serve my own web content.  
`EXPOSE 80`: means you want the container to receive web traffic through port 80.  
Basically, these three lines tell Docker to create an image using the pre-existing Nginx docker image and replace the index.html file with a new index.html file, and expose the web server to port 80.

### What is a Docker file?

The Dockerfile is a list of instructions for Docker to follow.

### Custom Image

To build the imate, I used the command: `docker build -t my-web-app .`

> ### Command break down:
>
> `docker build` builds the image, following the instructions defined in the dockerfile.  
>  `-t my-web-app` names the image "my-web-app".  
>  `.` tells Docker to find the Dockerfile in the current directory.

To run the image, I used the same command to run the Nginx image, but with a slight change.  
Instead of `docker run -d -p 80:80 nginx`, I used `docker run -d -p 80:80 my-web-app`, which would tell docker to run the image "my-web-app", not Nginx.  
However, since the container with Nginx was still running on port 80, an error appeared suggesting that a container was already using port 80.  
To stop the currently running container, I used the command `docker stop <container_id>`, with `<container_id>` being `nginx`.

In this instance, the container image is the instructions and the files, and the container is the running server and website used to host and distribute the contents, like the index.html file.

## Deploy the custom image to Elastic Beanstalk

In this step, I will deploy the custom Docker image to AWS Elastic Beanstalk.

### What is Elastic Beanstalk?

AWS Elastic Beanstalk is a service that makes it easy to deploy cloud applications without having to worry about the underlying infrastructure. By simply uploading code, Elastic Beanstalk will handle everything needed, like setting up servers and managing scaling, to get it running.

More specifically, Elastic beanstalk will handle things like:

- Capacity provisioning
- Load balancing
- Auto-scaling
- Application Health Monitoring

### Elastic Beanstalk application configuration:

**Configure Enironment: Web server environment**

**What is a web server environment?**  
When you configure your environemnt, you're deciding the instance type (the type of EC2 instance that will be running the conatiner), scaling options, network settings and more.

**EC2 and Elastic Beanstalks:**
Elastic Beanstalks use EC2 instances (i.e. virtual servers) under the hood to run an application.

**Application Name:** _NextWork App_

**Platform:** _Docker_

> **What is a platform?**  
> The platform is the technology that will open and run the application.

> **Platform branch** is the type of Docker a user will be using e.g. the default Docker running on 64bit Amazon Linux 2023 is the type of Docker that's optimised to be used on an Amazon Linux 2023 operating system.

**Application Code:** selected _Upload your code_

**Set up networking, database, and tags:**  
**Activate Public IP address**
This makes the EC2 instances in the application accessible from the internet.

**Configure instance traffic and scaling page:**  
 **Root volume type:** _General Purpose 3(SSD)_

> Root volume type is simply the EC2 instance's storage space.

**Instance metadata service (IMDS):** _Deactivated_

> IMDS is a service that gives applications temporary credentials to an EC2 instance. For example, you can use IMDS to give your application the permission to access S3 data.

Since the app won't need access to other AWS services, I won't need IMDS.
