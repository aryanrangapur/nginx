# Nginx

## Introduction to Nginx and Its Purpose
Nginx is a powerful web server with a non-blocking architecture designed to solve web server problems. It provides several benefits, including load balancing, HTTP caching, and reverse proxy capabilities.

 

## Key Benefits

### Load Balancing, Caching, and Reverse Proxy
- A normal HTTP connection involves a client directly connecting to a server.
- **Forward Proxy**: A VPN acts as a forward proxy by forwarding requests from clients to servers. Multiple clients communicate through one VPN server, illustrating forward proxy functionality.
- **Reverse Proxy**: Reverse proxy allows multiple servers to connect back to a single client. In this setup, the user does not know which server processes their request. Nginx is commonly used as a reverse proxy for many websites.

 

## How Does Nginx Work?

- Users do not communicate directly with the main server but through Nginx as an intermediary.
- Configuration files determine how requests are routed by Nginx based on various parameters.

 

## Nginx Features: Load Balancing and Caching

- When clients request data, Nginx forwards it based on its configuration settings.
- **Load Balancing**: Ensures no single server is overwhelmed by traffic using algorithms like round-robin.
- **HTTP Caching**: Improves response times for frequently requested resources like images.

 

## Basic Architecture of Nginx Server

- Discusses the basic architecture of an Nginx server and its configuration for handling multiple requests.
- Explains how requests starting with `/admin` are handled by the server, demonstrating request routing.
- Provides a real-life example using Google Maps to illustrate how reverse proxying works.

 

## Reverse Proxy Functionality

- Describes how requests to specific paths like `/maps` are routed to different servers via reverse proxy.
- Highlights that while Google may not use Nginx, similar reverse proxy configurations exist in their architecture.

 

## Benefits of Using Nginx

- **Concurrency**: Handles 10,000 concurrent requests efficiently.
- **Caching and Load Balancing**: Nginx can cache HTTP requests, enhancing performance for repeated queries. It also acts as a load balancer when multiple servers are present, distributing traffic effectively.
- **Static File Serving**: Serves and caches static files like images and videos directly from the file directory.

 

## SSL Certificates and API Gateway

- Nginx can manage SSL certificates for secure connections on websites.
- Introduces the concept of using Docker for development and testing environments related to Nginx configurations.

 

## Preparation for Using Nginx Effectively

- Emphasizes the importance of understanding Linux basics before diving into more complex setups with Nginx.
- Advises having a free cloud account set up for future practical applications involving virtual machines.
- Reiterates the need for knowledge in Docker and Linux basics as prerequisites for working with Nginx effectively.

 
---

## Moving from Theory to Practice: Setting Up Nginx

Alright, we've covered the foundational theory to get you started with Nginx. Of course, there’s a lot more to learn about Nginx's internal architecture and how it works under the hood. But for now, let’s roll up our sleeves and get hands-on. We'll delve deeper into the intricacies later (hopefully 😉).

### Setting Up an Ubuntu Machine with Docker

First, we'll use Docker to create an Ubuntu machine and get started with Nginx. Follow these steps:

1. **Run a Docker Command**  
   Open your terminal and execute the following command to launch an Ubuntu container:  

   ```bash
   docker run -it -p 8090:80 ubuntu

  Replace 8090 with your preferred port if needed. This command runs an interactive Ubuntu container and maps the host's port 8090 to the container's port 80.

2. **Install Nginx Inside the Container**
   Once you're inside the Ubuntu machine, install Nginx and a text editor like vim by running:

    ```bash
    apt-get update
    apt-get install nginx vim

3. **Explore the Configuration File**
    The main configuration file for Nginx is located at /etc/nginx/nginx.conf. This file controls how Nginx behaves, including request routing, load balancing, and more. To view or edit the file, use:
    
   ```bash
   vim /etc/nginx/nginx.conf

  Familiarizing yourself with this file is essential, as it’s where all the magic happens!
    

### Modifying and Testing the Nginx Configuration

Once you've installed Nginx, let's dive into some practical steps to customize its configuration.

1. **Backup the Original Configuration File**  
   It's always a good idea to back up the default configuration file before making any changes. To rename the file, use the `mv` command:

   ```bash
   mv /etc/nginx/nginx.conf /etc/nginx/nginx-backup.conf

This renames the default nginx.conf to nginx-backup.conf, allowing you to revert to the original settings if needed.

2. **Create a New Configuration File**
   Now, let's create a fresh nginx.conf file and add the following content:

   ```bash
   events {
       
   }
   
   http {
       # The 'http' block is where HTTP-related configurations are defined, such as servers, locations, and directives for handling HTTP requests.
   
       server {
           listen 80; 
          
           server_name _; 
           # Using '_' acts as a wildcard, meaning it accepts any hostname.
   
           location / {
               # The 'location' block defines how Nginx should handle requests to a specific path. Here, '/' refers to the root path.
   
               return 200 "Hello from Nginx conf file"; 
           }
       }
   }
   ```
   This configuration sets up a simple HTTP server that responds to any request to the root path (/) with a 200 OK status and the message Hello from Nginx conf file.

3. **Test the Configuration**
   Before applying the changes, test the new configuration to ensure there are no syntax errors:

   ```bash
    nginx -t
   ```
   If everything is correct, you should see a message like:
   ```bash
   nginx: configuration file /etc/nginx/nginx.conf test is successful
   ```
   Reload Nginx
   Apply the new configuration without restarting the server by reloading Nginx:
   ```bash
   nginx -s reload
   ```
   Verify the Changes
   Open your browser and navigate to http://localhost:8090. You should see the message:
   ```bash
   Hello from Nginx conf file
   ```


## Serving Static Files Using Nginx

Now that you've successfully configured Nginx to return a simple message, let's move on to serving static files, such as HTML and CSS, using Nginx.

### 1. Create a Folder for Your Website Files
  First, create a folder to hold your website files. For this example, we'll use the `/etc/nginx/website` directory:

```bash
mkdir /etc/nginx/website
```
Inside this folder, create your basic index.html and style.css files.

### 2. Modify the Nginx Configuration
Now, modify the Nginx configuration file to serve these static files. Update your nginx.conf file like this:

```bash
events {
    # The 'events' block remains empty as no special event handling is needed.
}

http {
    include /etc/nginx/mime.types;
    # 'mime.types' is included to help Nginx recognize file types like HTML, CSS, JS, etc.

    server {
        listen 80;
        server_name _;

        root /etc/nginx/website;
        # The 'root' directive tells Nginx where your static files are located.
        # Here, it points to /etc/nginx/website where the index.html and style.css are stored.
    }
}
```

### 3. Test and Reload the Configuration
  As always, before reloading the Nginx service, test the configuration to ensure it's correct:
```bash
nginx -t
```
If the test is successful, reload Nginx to apply the changes:

```bash
nginx -s reload
```
### 4. Access Your Static Site
Now, open your browser and navigate to http://localhost:8090. You should see your simple static webpage with the styles applied from style.css.

If you set everything up correctly, the browser will display the page with a heading "Welcome to My Static Website" and a paragraph styled with the CSS file.
