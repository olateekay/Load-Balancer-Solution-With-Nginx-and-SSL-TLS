# Load Balancer Solution With Nginx and SSL/TLS

In this project we will configure an Nginx Load Balancer solution.

It is also extremely important to ensure that connections to your Web solutions are secure and information is encrypted in transit - we will also cover connection over secured HTTP (HTTPS protocol), its purpose and what is required to implement it.

Also,In this project you will register your website with LetsEnrcypt Certificate Authority, to automate certificate issuance you will use a shell client recommended by LetsEncrypt - cetrbot.

![alt text](image1.jpg)

## Task
This project consists of two parts:

1. Configure Nginx as a Load Balancer

2. Register a new domain name and configure secured connection using SSL/TLS certificates


## Configure Nginx as a Load Balancer

a. Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 - this port is used for secured HTTPS connections)
b. Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses
c. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

```
sudo apt update
sudo apt install nginx

```
Ensure nginx is running and enable it across reboots

```
sudo systemctl start nginx && sudo systemctl enable nginx

sudo systemctl status nginx

```

d. Create a config file for nginx, 

`sudo vi /etc/nginx/sites-available/load_balancer.conf`

## Register a new domain name and configure secured connection using SSL/TLS certificates

1. Register on my.freenom.com and create a new domain name

2. Go to your AWS console on Route 53 dashboard, create a hosted zone and paste your new domain name in the domain name tab


![alt text](image1.jpg)

3. For your Route 53 hosted zone to be connected to your domain name copy the name servers from route 53 to the free domain we just created




```

```
