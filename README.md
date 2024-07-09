# https-RPC
how to set up a https RPC, using NGINX and Certbot.

## Update System Packages
Frist, ensure that all packages are up to date. This can prevent security vulnerabilities.

```sudo apt update && sudo apt upgrade -y && sudo apt autoremove```

## Install Monitoring and Web Server Tools.
```sudo apt install -y glances```

## Install Nginx: A high-performance web server and a reverse proxy, often used for load balancing.
```sudo apt install -y nginx```

## Configure UFW Firewall for Nginx: Make sure Nginx can receive HTTP and HTTPS traffic.
```
sudo ufw enable && \
sudo ufw allow 'Nginx HTTP' && \
sudo ufw allow 'Nginx HTTPS' && \
sudo ufw allow 22
```

## Intall Certbot.
```sudo apt-get install -y certbot python3-certbot-nginx```

## setup Certbot with your domain.
```
sudo certbot certonly --manual --preferred-challenges dns \
--email <your-email-address> \
-d <your-domain.abc>
```

## Set Up Nginx Configuration
Remove Existing Nginx Configuration:
```sudo rm -rf /etc/nginx/nginx.conf```

Edit New Configuration: Use a text editor like nano to create a new configuration file.

```sudo nano /etc/nginx/nginx.conf```

- Replace the contents with your load balancer configuration. You can use the [Example Nginx config](https://github.com/amis13/https-RPC/blob/main/example-nginx.config) as a starting point.

- Modify RPC/LCD/gRPC server entries and domain names as required.

## Test Nginx Configuration: Ensures your syntax is correct.

```sudo nginx -t```

## Enable and Start Nginx: This will make sure Nginx starts on boot and starts running immediately.
```sudo systemctl enable nginx && sudo systemctl start nginx```

## Finally, add a cronjob to crontab to enable auto-newing of the certificates:
```
crontab -e
```
Then, add:
```
0 12 * * * /usr/bin/certbot renew --quiet
```
