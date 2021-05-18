# Load Balancer Solution With Nginx and SSL/TLS

In this project we will configure an Nginx Load Balancer solution.

It is also extremely important to ensure that connections to your Web solutions are secure and information is encrypted in transit - we will also cover connection over secured HTTP (HTTPS protocol), its purpose and what is required to implement it.

Also,In this project you will register your website with LetsEnrcypt Certificate Authority, to automate certificate issuance you will use a shell client recommended by LetsEncrypt - cetrbot.

![alt text](image1.jpg)

## Task
This project consists of two parts:

1. Configure Nginx as a Load Balancer

2. Register a new domain name and configure secured connection using SSL/TLS certificates


## Register a new domain name and configure secured connection using SSL/TLS certificates

1. Register on my.freenom.com and create a new domain name

2. Go to your AWS console on Route 53 dashboard, create a hosted zone and paste your new domain name in the domain name tab


![alt text](image1.jpg)

3. For your Route 53 hosted zone to be connected to your domain name copy the name servers from route 53 to the free domain we just created

4.Spin up your EC2 instance, copy the public ip ,go to your Route53 create a record and paste the public ip to the value tab and create the record. create a second record for www.yourdomainname

These steps will ensure a secured handshake between the Nginx Load Balancer, Cloud DNS and the Web Servers.

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

```
#insert following configuration into http section

upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }
server {
    listen 80;
    server_name thecodingmun.tk www.thecodingmum.tk;
    location / {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass http://myproject;
    }
  }

#comment out this line
#       include /etc/nginx/sites-enabled/*;
```

d. Remove the default nginx config file;
`sudo rm -rf /etc/nginx/sites-enabled/default`

e. link the sites-avalaible and sites-enabled together

`sudo ln -s /etc/nginx/sites-available /etc/nginx/sites-enabled`

f. Test that your NGINX configuration is correct
 `sudo nginx -t`

g. Now reload Nginx 
`sudo systemctl reload nginx`

## Install Certbot

`sudo apt install certbot`

Also install a certbot dependency

`sudo apt install python3-certbot-nginx -y`

Request for a TLS certificate for your domain:
`sudo certbot --nginx -d thecodingmum.tk -d www.thecodingmum.tk`

Your website will load with the HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string. Click on the padlock icon and you can see the details of the certificate issued for your website

By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.
Best pracice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day.

To do so, lets edit the crontab file with the following command:

`crontab -e`
Add following line:

`* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1`
You can always change the interval of this cronjob if twice a day is too often by adjusting schedule expression.

You have just implemented an Nginx Load Balancing Web Solution with secured HTTPS connection with periodically updated SSL/TLS certificates.